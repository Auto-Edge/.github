# AutoEdge

Deploy ML models to iOS devices without App Store updates.

## The Problem

You trained a PyTorch model. Now you want it running on iPhones. The typical path: is:

1. Convert to CoreML manually
2. Bundle into your app
3. Submit to App Store review (1-7 days)
4. Users update the app
5. Repeat for every model improvement

## Here is How AutoEdge Fixes It

**Upload** your PyTorch model → **Convert** to CoreML (optimized, 50% smaller) → **Publish** to the registry → iOS devices **download automatically**.

https://github.com/user-attachments/assets/05cea64a-1368-4594-bb30-71459a2b8e5a

## Key Features

- **One-click conversion**: PyTorch → CoreML with FP16 quantization
- **OTA delivery**: Push model updates directly to devices
- **Version control**: Publish, rollback, A/B test different versions
- **Analytics**: Track inference latency, downloads, and device types
- **Offline-first**: Models cache on-device, run without network

## Who It's For

- Mobile ML engineers shipping models to iOS
- Teams iterating quickly on model improvements
- Apps that need on-device inference (privacy, speed, offline)

## Demo

```bash
git clone https://github.com/Auto-Edge/autoedge-stack.git
cd autoedge-stack
chmod +x bootstrap.sh
sudo ./bootstrap.sh
docker compose up --build
```

1. Convert a demo model (MobileNetV2)
2. Publish it in the registry
3. Use AutoSDK in any iOS app (Instructions below).
4. See inference metrics in the dashboard

```bash
// In Xcode: File > Add Package Dependencies
// Enter the repository URL:
https://github.com/Auto-Edge/ios-sdk

// Or add to Package.swift:
dependencies: [
    .package(url: "https://github.com/Auto-Edge/ios-sdk", from: "1.0.0")
]
```

```bash
import AutoEdgeSDK

// Configure on app launch
AutoEdgeSDK.shared.configure(
    serverURL: URL(string: "http://localhost:8000")!
)

// Load a model
let modelURL = try await AutoEdgeSDK.shared.loadModel(
    modelId: "my-model"
)

// Use with Core ML
let model = try MLModel(contentsOf: modelURL)

// Report inference metrics
AutoEdgeSDK.shared.reportInference(
    modelId: "my-model",
    latencyMs: 15.5
)
```


