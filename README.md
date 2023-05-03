# MLC LLM


<img src="site/img/diag.svg" alt="Architecture Diagram" height=""/>

MLC LLM is a **universal solution** that allows **any language models** to be **deployed natively** on a diverse set of hardware backends and native applications, plus a **productive framework** for everyone to further optimize model performance for their own use cases.


<p align="center">
  <img src="site/demo.gif" height="700">
</p>

## What is MLC LLM?

In recent years, there has been remarkable progress in generative artificial intelligence (AI) and large language models (LLMs), which are becoming increasingly prevalent. Thanks to open-source initiatives, it is now possible to develop personal AI assistants using open-sourced models. However, LLMs tend to be resource-intensive and computationally demanding. To create a scalable service, developers may need to rely on powerful clusters and expensive hardware to run model inference. Additionally, deploying LLMs presents several challenges, such as their ever-evolving model innovation, memory constraints, and the need for potential optimization techniques.

The goal of this project is to enable the development, optimization, and deployment of AI models for inference across a range of devices, including not just server-class hardware, but also users' browsers, laptops, and mobile apps. To achieve this, we need to address the diverse nature of compute devices and deployment environments. Some of the key challenges include:

- Supporting different models of CPUs, GPUs, and potentially other co-processors and accelerators.
- Deploying on the native environment of user devices, which may not have python or other necessary dependencies readily available.
- Addressing memory constraints by carefully planning allocation and aggressively compressing model parameters.

MLC LLM offers a repeatable, systematic, and customizable workflow that empowers developers and AI system researchers to implement models and optimizations in a productivity-focused, Python-first approach. This methodology enables quick experimentation with new models, new ideas and new compiler passes, followed by native deployment to the desired targets. Furthermore, we are continuously expanding LLM acceleration by broadening TVM backends to make model compilation more transparent and efficient.

## How does MLC Enable Universal Native Deployment?

The cornerstone of our solution is machine learning compilation ([MLC](https://mlc.ai/)), which we leverage to efficiently deploy AI models. We build on the shoulders of open-source ecosystems, including tokenizers from HuggingFace and Google, as well as open-source LLMs like Llama, Vicuna, Dolly, MOSS and more. Our primary workflow is based on [Apache TVM Unity](https://github.com/apache/tvm/tree/unity), an exciting ongoing development in the Apache TVM Community.

- Dynamic shape: We bake a language model as a TVM IRModule with native dynamic shape support, avoiding the need for extra padding to the maximum length and reducing both computation amount and memory usage.
- Composable ML compilation optimizations: we perform many model deployment optimizations, such as better compilation code transformation, fusion, memory planning, library offloading and manual code optimization can be easily incorporated as TVM's IRModule transformations exposed as Python APIs.
- Quantization: We utilize low-bit quantizations to compress the model weights and leverage TVM's loop-level TensorIR to quickly customize code generations for different compression encoding schemes.
- Runtime: The final generated libraries run on the native environment, with TVM runtime that comes with minimal dependencies, which supports various GPU driver APIs and native language bindings (C, JavaScript, etc).


Additionally, we also provide a lightweight C++-based example CLI app that showcases how to wrap up the compiled artifacts and necessary pre/post-processing, which will hopefully clarify the workflow to embed them into native applications.

As a starting point, MLC generates GPU shaders for CUDA, Vulkan and Metal. It is possible to add more support, such as OpenCL, sycl, webgpu-native, through improvements to TVM compiler and runtime. MLC also supports various CPU targets including ARM and x86 via LLVM.

We heavily rely on open-source ecosystem, more specifically, [TVM Unity](https://discuss.tvm.apache.org/t/establish-tvm-unity-connection-a-technical-strategy/13344), an exciting latest development in the TVM project that enables python-first interactive MLC development experiences that allows us to easily compose new optimizations all in Python, and incrementally bring our app to the environment of interest. We also leveraged optimizations such as fused quantization kernels, first class dynamic shape support and diverse GPU backends.

