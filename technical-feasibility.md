* The current intermediate solution demonstrates the feasibility of a Cloud–Far Edge MLOps deployment architecture, simulated by a GPU-enabled Kubernetes cluster on the Cloud side and a Jetson Orin embedded node on the Far Edge side.

* The Jetson Orin runs the local execution layer:
  * systemd supervision,
  * Local Rust Pipeline Runner,
  * WasmEdge `*.wasm` modules,
  * TorchScript `model.pt`,
  * local artifact cache,
  * opportunistic data upload.

* The Cloud layer relies on a Kubernetes-based MLOps/data stack, including:
  * Kubernetes, MLflow, ArgoCD and GitLab Runner;
  * Label Studio, MinIO/S3, Harbor/OCI registry and object storage;
  * PyTorch-based retraining and fine-tuning, ONNX Runtime-based validation/inference, MLServer/KServe serving, and model lifecycle management services (such as publication).

* The Far Edge layer is covered by Jetson Orin AGX-class hardware, suitable for edge inference simulation with up to 64 GB unified LPDDR5 memory; the DINOv3 ViT-S/16 profile is positioned for Edge inference with a 1–4 GB memory footprint.

* The GPU cluster can support training/retraining/fine-tuning bursts by reserving a large GPU profile; the available cluster capacity includes H100 NVL-class GPUs with around 94 GB VRAM, suitable for 40–80 GB VRAM workloads. This capacity is aligned with the DINOv3 ViT-7B/16 profile, which requires 40–80 GB VRAM for large-scale training/retraining/fine-tuning workloads on A100/H100-class GPUs.

* The Far Edge-side inference use case is based on reusing the Inference Service provided by an existing inference service.

* Cloud <-> Far Edge exchanges are simulated within an isolated test environment (DMZ) over standard HTTP on a wired Jetson link, with internal SSH port-forwarding only, and no external SSH tunnel or SOCKS proxy.
