# Hugging Face GraphQL Schema

## Overview

This conceptual GraphQL schema models the Hugging Face AI platform, covering the full surface area of its public APIs: the Hub API for managing models, datasets, and spaces; the Inference API for running ML pipelines; Inference Endpoints for dedicated deployments; and the Dataset Viewer API for querying hosted datasets.

Hugging Face does not currently expose a native GraphQL endpoint. This schema is a conceptual translation of its REST APIs into a unified GraphQL type system, enabling developers to reason about Hugging Face resources as a connected graph and to design GraphQL gateways, federation layers, or tooling on top of the underlying REST surface.

## Source APIs

- **Hub API** — `https://huggingface.co/api` — repository management, user and org operations, commits, branches, pull requests, discussions
- **Inference API** — `https://api-inference.huggingface.co` — serverless ML inference across NLP, vision, audio, and multimodal tasks
- **Inference Endpoints API** — `https://api.endpoints.huggingface.cloud` — dedicated endpoint lifecycle management
- **Dataset Viewer API** — `https://datasets-server.huggingface.co` — split enumeration, row preview, search, and statistics

## Type Inventory

### Repository and Hub Types

| Type | Description |
|------|-------------|
| `Model` | A machine learning model repository on the Hub |
| `ModelInfo` | Detailed metadata for a model including pipeline tag and card data |
| `ModelFile` | A single file within a model repository |
| `ModelCard` | Structured metadata parsed from a model's README card |
| `ModelEvaluation` | Evaluation results associated with a model |
| `Dataset` | A dataset repository on the Hub |
| `DatasetInfo` | Detailed metadata for a dataset |
| `DatasetCard` | Structured metadata from a dataset's README card |
| `DatasetContent` | Paginated row content from a dataset split |
| `Space` | A Gradio or Streamlit application hosted on the Hub |
| `SpaceInfo` | Detailed metadata for a Space |
| `SpaceCard` | Structured metadata from a Space's README card |
| `SpaceRuntime` | Current runtime state and hardware of a Space |
| `Repository` | Generic repository abstraction shared by models, datasets, and spaces |
| `Branch` | A named branch within a Hub repository |
| `Tag` | A named tag pointing to a specific commit |
| `Commit` | A single commit in a repository's history |
| `CommitInfo` | Full metadata for a commit including author and changed files |
| `PullRequest` | A pull request opened against a Hub repository |
| `Review` | A review submitted on a pull request |
| `Discussion` | A discussion thread on a model, dataset, or space |
| `Comment` | A comment within a discussion or pull request |
| `Webhook` | A webhook registered for Hub events |

### Identity and Access Types

| Type | Description |
|------|-------------|
| `User` | A registered Hugging Face user account |
| `UserProfile` | Extended public profile data for a user |
| `Organization` | A Hugging Face organization grouping users and resources |
| `OrgMember` | A user's membership record within an organization |
| `OrgResource` | A resource (model, dataset, space) owned by an organization |
| `Token` | A user or application token used for API authentication |
| `AccessToken` | A scoped access token with read or write permissions |
| `APIKey` | An API key for third-party provider access |

### Inference and Pipeline Types

| Type | Description |
|------|-------------|
| `InferenceRequest` | Input payload for a serverless inference call |
| `InferenceOutput` | Output payload returned from an inference call |
| `Pipeline` | An ML pipeline task type supported by the Inference API |
| `TextGeneration` | Output from a text generation pipeline |
| `TextClassification` | Label and score output from a text classification pipeline |
| `TokenClassification` | Per-token label output from a token classification pipeline |
| `NER` | Named entity recognition output with entity spans |
| `QuestionAnswering` | Answer span and score from a question-answering pipeline |
| `SummarizationResult` | Generated summary from a summarization pipeline |
| `Translation` | Translated text from a translation pipeline |
| `FeatureExtraction` | Embedding vector from a feature extraction pipeline |
| `ImageClassification` | Label and confidence from an image classification pipeline |
| `ObjectDetection` | Bounding box and label from an object detection pipeline |
| `Segmentation` | Mask and label from a segmentation pipeline |
| `DepthEstimation` | Depth map from a monocular depth estimation pipeline |
| `AudioClassification` | Label and score from an audio classification pipeline |
| `SpeechRecognition` | Transcribed text from an ASR pipeline |
| `TextToSpeech` | Audio blob output from a TTS pipeline |
| `ZeroShotClassification` | Candidate label scores from a zero-shot pipeline |
| `TableQuestion` | Answer extracted from a table by a table-QA pipeline |
| `FillMask` | Top token candidates from a masked language model |
| `Document` | Input document payload for document understanding tasks |

### Endpoint and Deployment Types

| Type | Description |
|------|-------------|
| `InferenceEndpoint` | A dedicated, scalable inference endpoint |
| `EndpointStatus` | Current status and replica count of an endpoint |
| `EndpointScaling` | Autoscaling configuration for an endpoint |
| `EndpointCompute` | Hardware and instance type configuration for an endpoint |

### Dataset Viewer Types

| Type | Description |
|------|-------------|
| `DatasetSplit` | A named split (train/validation/test) within a dataset |
| `DatasetRow` | A single data row from a dataset split |
| `DatasetColumn` | Schema and type information for a dataset column |
| `DatasetStatistics` | Column-level statistics for a dataset split |
| `DatasetSearchResult` | Matching rows from a full-text dataset search |
| `ParquetFile` | A Parquet shard file for a dataset split |

## Query Surface

The schema exposes the following top-level query fields:

- `model(id: ID!)` / `models(filter: ModelFilter, limit: Int, offset: Int)` — fetch models by ID or list with filters
- `dataset(id: ID!)` / `datasets(filter: DatasetFilter, limit: Int, offset: Int)` — fetch datasets
- `space(id: ID!)` / `spaces(filter: SpaceFilter, limit: Int, offset: Int)` — fetch spaces
- `user(username: String!)` / `me` — fetch user profile or authenticated user
- `organization(name: String!)` — fetch organization details
- `inferenceEndpoint(namespace: String!, name: String!)` — fetch a single endpoint
- `inferenceEndpoints(namespace: String!)` — list endpoints for a namespace
- `datasetSplits(dataset: String!)` — list available splits
- `datasetRows(dataset: String!, split: String!, offset: Int, length: Int)` — paginated rows

## Mutation Surface

- `createRepository(input: CreateRepositoryInput!)` — create a model, dataset, or space repo
- `deleteRepository(id: ID!, type: RepositoryType!)` — delete a repository
- `updateRepositoryVisibility(id: ID!, private: Boolean!)` — toggle visibility
- `createCommit(input: CreateCommitInput!)` — commit files to a repository
- `createPullRequest(input: CreatePullRequestInput!)` — open a pull request
- `mergePullRequest(id: ID!)` — merge an open pull request
- `createDiscussion(input: CreateDiscussionInput!)` — open a discussion thread
- `addComment(discussionId: ID!, body: String!)` — reply to a discussion
- `createInferenceEndpoint(input: CreateEndpointInput!)` — provision a dedicated endpoint
- `updateInferenceEndpoint(name: String!, input: UpdateEndpointInput!)` — update endpoint config
- `deleteInferenceEndpoint(namespace: String!, name: String!)` — delete an endpoint
- `pauseInferenceEndpoint(namespace: String!, name: String!)` — pause a running endpoint
- `resumeInferenceEndpoint(namespace: String!, name: String!)` — resume a paused endpoint
- `runInference(input: InferenceRequest!)` — run serverless inference
- `createWebhook(input: CreateWebhookInput!)` — register a webhook
- `deleteWebhook(id: ID!)` — remove a webhook

## References

- Hub API Documentation: https://huggingface.co/docs/hub/api
- Inference API Documentation: https://huggingface.co/docs/api-inference/index
- Inference Endpoints Documentation: https://huggingface.co/docs/inference-endpoints/api_reference
- Dataset Viewer Documentation: https://huggingface.co/docs/dataset-viewer/index
- GitHub Organization: https://github.com/huggingface
- Hub Python SDK: https://huggingface.co/docs/huggingface_hub/index
