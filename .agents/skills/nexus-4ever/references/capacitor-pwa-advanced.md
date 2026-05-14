# Capacitor + PWA Advanced Patterns

## Purpose

Advanced patterns for hybrid mobile apps built with Capacitor or PWA. Covers native plugin integration, build configuration, camera/barcode access, deep links, and Android-specific rules.

Use when `frontend.mobile.driver: capacitor` or `frontend.mobile.driver: pwa` in `NEXUS_CONFIG.md`.

---

## Core Rules

### 1. capacitor.config.ts — Required Baseline

```typescript
// capacitor.config.ts
import type { CapacitorConfig } from '@capacitor/cli';

const config: CapacitorConfig = {
  appId: 'com.yourcompany.appname',   // reverse domain — must be unique
  appName: 'App Name',
  webDir: 'dist',                      // output of your build command
  server: {
    androidScheme: 'https',            // ✅ always HTTPS — security requirement
  },
  plugins: {
    SplashScreen: {
      launchShowDuration: 2000,
      backgroundColor: '#0d1117',
      showSpinner: false,
    },
    Camera: {
      // permissions declared here, not just in AndroidManifest
    },
  },
};
export default config;
```

### 2. Camera / Barcode Scanner

```typescript
import { Camera, CameraResultType, CameraSource } from '@capacitor/camera';
import { BarcodeScanner } from '@capacitor-community/barcode-scanner';

// ✅ Always request permissions before use
async function scanBarcode(): Promise<string | null> {
  const permission = await BarcodeScanner.checkPermission({ force: true });

  if (!permission.granted) {
    console.warn('Camera permission denied');
    return null;
  }

  await BarcodeScanner.hideBackground();
  const result = await BarcodeScanner.startScan();
  await BarcodeScanner.showBackground();

  return result.hasContent ? result.content : null;
}

// ✅ Always stop scanning on component unmount
useEffect(() => {
  return () => {
    BarcodeScanner.stopScan();
    BarcodeScanner.showBackground();
  };
}, []);
```

### 3. Deep Link Handling

```typescript
import { App } from '@capacitor/app';

// ✅ Register deep link handler on app init — not inside components
App.addListener('appUrlOpen', (event) => {
  const url = new URL(event.url);

  // Route based on path
  if (url.pathname.startsWith('/orders/')) {
    const orderId = url.pathname.split('/').pop();
    router.push(`/orders/${orderId}`);
  }
});

// AndroidManifest.xml — declare intent filter
// <intent-filter android:autoVerify="true">
//   <action android:name="android.intent.action.VIEW" />
//   <category android:name="android.intent.category.DEFAULT" />
//   <category android:name="android.intent.category.BROWSABLE" />
//   <data android:scheme="https" android:host="your-domain.com" />
// </intent-filter>
```

### 4. PWA Manifest Requirements

```json
// public/manifest.json
{
  "name": "App Full Name",
  "short_name": "App",
  "description": "App description for install prompt",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#0d1117",
  "theme_color": "#6366f1",
  "orientation": "portrait",
  "icons": [
    { "src": "/icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icons/icon-512.png", "sizes": "512x512", "type": "image/png", "purpose": "maskable" }
  ]
}
```

### 5. Native vs Web Capability Detection

```typescript
import { Capacitor } from '@capacitor/core';

// ✅ Always check before using native APIs
const isNative = Capacitor.isNativePlatform();
const platform = Capacitor.getPlatform(); // 'ios' | 'android' | 'web'

async function takePicture() {
  if (!isNative) {
    // Web fallback — use file input
    fileInputRef.current?.click();
    return;
  }
  // Native: use Capacitor Camera plugin
  const photo = await Camera.getPhoto({
    quality: 80,
    resultType: CameraResultType.DataUrl,
    source: CameraSource.Camera,
  });
  return photo.dataUrl;
}
```

---

## Android Build Checklist

- [ ] `capacitor.config.ts` uses `androidScheme: 'https'`
- [ ] All required permissions declared in `AndroidManifest.xml`
- [ ] `appId` is unique reverse-domain format
- [ ] Icons provided at 192x192 and 512x512 (maskable)
- [ ] `npx cap sync` run after every web build
- [ ] Deep links configured with `autoVerify: true`
- [ ] Camera/Scanner cleanup on component unmount
- [ ] Native capability detection before any plugin call
