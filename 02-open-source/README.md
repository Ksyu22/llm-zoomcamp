# Module & Notebooks Overview

## Running Ollama Locally
`ollama_locally.ipynb`

In this notebook, I interacted with the Ollama server launched locally on my CPU and within Docker. Due to RAM limitations, I used the `gemma:2b` model.
There are multiple ways to call Ollama, including LlamaIndex, Langchain, and the native Python library, among others.

## RAG with Ollama & Elasticsearch
`ollama_elastic_docker_compose.ipynb`

Both services were run using Docker Compose. The main challenges included fitting everything within a limited amount of RAM and managing CPU resources effectively.

To address these issues, I adjusted the following aspects of the application:
- Used a smaller model like `gemma:2b`.
- Constrained Docker containers with `cpus` and `mem_limit` options to limit the maximum usage of memory and CPU per service.

Although these techniques allowed me to obtain results, the output quality was poor. Specifically, the output did not align with the ground truth available in the documents, and the text did not sound like it was written by a native English speaker.


## Exploring Model Weights Management in Docker

In the notebook `ollama_h.ipynb`, I explored model weights management within Docker.

There are several approaches to manage model weights in Docker:

1. **Including Weights in the Docker Image:**
    - ✅ **Advantages:**
        - Weights are directly included, so no need for additional management.
        - Simplifies deployment and distribution as everything is packed together.
    - ❌ **Disadvantages:**
        - The Docker image can become very large, which slows down building and deployment.
        - Updating weights requires rebuilding and redeploying the image.

2. **Downloading Weights at Container Start:**
    - ✅ **Advantages:**
        - No large weights are included in the image.
        - Weights are always up-to-date.
    - ❌ **Disadvantages:**
        - Slow startup time due to downloading weights each time the container starts.
        - Dependent on network access and potential downloading issues.

3. **Mapping Volumes:**
    - **Purpose:** Volumes are used to persist data across Docker containers and allow data sharing between containers and between containers and the host.
    - ✅ **Advantages:**
        - The Docker image size is smaller.
        - Weights can be updated independently from the Docker image.
        - Data persists across containers.
    - ❌ **Disadvantages:**
        - Requires managing volumes on the host system.

4. **Hybrid Approach:**
    - **Description:** Includes default weights in the Docker image while allowing updates or overrides using volumes.
    - ✅ **Advantages:**
        - Weights can be updated.
        - Updated weights are available across containers.
        - Functionality remains even without an internet connection.
    - ❌ **Disadvantages:**
        - The Docker image can become very large, slowing down building and deployment.
        - More complex setup.

For this usage, the mapping volumes approach was applied.
