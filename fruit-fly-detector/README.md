# Bactrocera Fruit Fly Detector (browser demo)

A static, client-side web page that runs the YOLOv8 fruit fly detection model
(from [bactrocera-fruitfly-yolov8-detection](https://github.com/sanaullahongit/bactrocera-fruitfly-yolov8-detection))
directly in the visitor's browser using [onnxruntime-web](https://github.com/microsoft/onnxruntime).
No server or backend is needed — everything, including the uploaded image, stays on the visitor's machine.

## Files

- `index.html` — the entire app (HTML, CSS, and JS in one file). Loads `onnxruntime-web` from a CDN
  and `model.onnx` from the same folder.
- `model.onnx` — the trained model (`paper1_trained_model.pt`), exported to ONNX format at 224x224
  input resolution. Verified to produce identical detections to the original PyTorch model before export.

## How it works

1. The uploaded image is letterbox-resized to 224x224 (matching how the model was trained/validated).
2. The resized image is run through the ONNX model directly in the browser (WASM backend).
3. The raw output (box coordinates + per-class scores for ~1,029 candidate detections) is filtered by
   confidence, non-max-suppressed, and mapped back to the original image's coordinates.
4. Boxes, species labels, and confidence scores are drawn on a canvas over the image.

## Deploying to sanaullahongit.github.io

This folder is meant to be added as a subfolder inside the `sanaullahongit.github.io` repository
(not replace anything already there), so it will be reachable at:

```
https://sanaullahongit.github.io/fruit-fly-detector/
```

To add it:

```bash
# from inside a local clone of sanaullahongit.github.io
cp -r /path/to/fruit-fly-detector ./fruit-fly-detector
git add fruit-fly-detector
git commit -m "Add fruit fly detector demo"
git push
```

## Limitations

The model was trained and validated on trap-camera-style images of two species only
(*B. dorsalis* and *B. zonata*, from a public dataset). It has not been validated on other photo
types (e.g. pinned specimens, studio close-ups) and detection quality will vary outside its
training domain — this is noted directly on the page for visitors.
