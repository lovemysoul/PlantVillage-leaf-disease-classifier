# Plant Disease Classification with Deep Learning

A plant-leaf disease classifier trained on the **PlantVillage** dataset. This project compares two transfer-learning architectures—**EfficientNetB0** and **MobileNetV2**—and explores practical deployment techniques including quantization, pruning, and an interactive Gradio interface.

> Replace the repository link below once the project is published: `[Project repository](<YOUR_REPOSITORY_URL>)`

## Overview

The classifier predicts one of 38 PlantVillage crop/disease classes from a leaf image. EfficientNetB0 delivered the strongest reported test result, while MobileNetV2 provides a lighter comparison model. The project also evaluates post-training quantization and unstructured pruning of the EfficientNetB0 model.

## Dataset

| Item | Value |
| --- | --- |
| Dataset | PlantVillage |
| Classes | 38 |
| Images | 54,305 |
| Image size | 256 × 256 pixels |

## Models and reported results

| Model / experiment | Test accuracy | Saved model size | CPU throughput |
| --- | ---: | ---: | ---: |
| EfficientNetB0 | **99.5456%** | 18.17 MB | 769.74 images/sec* |
| MobileNetV2 | 99.2263% | — | — |
| Quantized EfficientNetB0 | 99.5456% | **16.23 MB** | 14.47 images/sec |
| Pruned EfficientNetB0 | 99.5088% | 18.16 MB | Not measured |

\*The original-model throughput was measured with an earlier benchmark setup. It should not be directly compared with the quantized CPU figure unless device, batch size, and benchmarking procedure are identical.

### Optimization findings

- **Quantization** reduced the saved EfficientNetB0 model from 18.17 MB to 16.23 MB (about 10.7%) with the same reported test accuracy.
- **Pruning** retained near-baseline accuracy, with a 0.0368 percentage-point decrease, but unstructured pruning did not materially reduce the dense checkpoint size.

## Gradio demo

The project includes a Gradio interface that accepts an uploaded leaf image and returns the **top three predictions** with confidence scores.

When launched from Google Colab with sharing enabled, Gradio supplies a temporary `gradio.live` URL. That URL expires when the Colab runtime or sharing session ends; add a permanent hosted demo URL here if one is deployed.

An internet-sourced test image returned:

| Rank | Prediction | Confidence |
| ---: | --- | ---: |
| 1 | Strawberry — Leaf scorch | 27% |
| 2 | Grape — Black rot | 20% |
| 3 | Corn (maize) — Common rust | 14% |

The low top confidence (27%) is an example of why the interface should treat uncertain predictions cautiously—ideally with a confidence warning or request for a clearer, in-domain image.

## Run in Google Colab

1. Open the project notebook in Google Colab (or upload the notebook and project files to your Drive/runtime).
2. Select a GPU runtime if the notebook includes training or GPU inference.
3. Make the PlantVillage dataset available in the location expected by the notebook.
4. Run the notebook cells in order to load data, train or load model weights, evaluate, and launch the demo.
5. For the Gradio cell, enable sharing if needed and open the temporary link printed by Colab.

The exact package versions, notebook names, dataset path, checkpoint filenames, and launch command are intentionally not prescribed here because they should match the files in your copy of the project. Add those concrete details after publishing the repository.

## Typical workflow

1. Prepare PlantVillage images and class labels.
2. Fine-tune EfficientNetB0 and MobileNetV2 using PyTorch.
3. Evaluate the models on the held-out test split.
4. Apply and evaluate quantization and pruning for EfficientNetB0.
5. Launch Gradio and upload a leaf image to view the top-three predictions.

## Limitations

- **PlantVillage domain:** PlantVillage images are relatively controlled. Strong test-set accuracy does not establish the same performance for field photographs with varied lighting, backgrounds, occlusion, blur, or multiple leaves.
- **Closed-set predictions:** The model always selects one of its 38 known classes, including for unseen species, diseases, or non-leaf images.
- **Confidence is not a diagnosis:** The 27% internet-image example shows that a predicted label can be uncertain. A confidence threshold and user-facing warning would make the demo safer to interpret.
- **Deployment measurements:** Throughput values collected under different devices or batch settings are not directly comparable.
- **Pruning format:** Unstructured pruning can create zeros without reducing the size of a standard dense saved checkpoint. Sparse-aware export or structured pruning may be needed for size or latency gains.

## Future improvements

- Validate on diverse, real-world field images.
- Add confidence thresholds, out-of-distribution checks, and clearer user guidance.
- Benchmark all models under the same hardware and inference configuration.
- Explore structured pruning, mobile-oriented export, and permanent demo hosting.

## Project status

This project demonstrates high reported classification accuracy on the PlantVillage test set, compares EfficientNetB0 and MobileNetV2, and documents deployment-oriented experiments. It should be treated as a research/learning prototype—not as a standalone agricultural diagnosis tool.

