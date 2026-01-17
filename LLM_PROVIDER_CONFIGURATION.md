# LLM Provider Configuration Guide

This document explains how to configure different Language Model providers for the home-ai-assistant backend. The system supports multiple providers including Azure OpenAI, OpenAI, and Local OpenAI models.

## Supported Providers

The system supports three main LLM providers:

1. **Azure OpenAI** - Microsoft's cloud-based OpenAI service
2. **OpenAI** - Standard OpenAI service (including hosted models)
3. **Local OpenAI** - Local inference using services like LocalAI

## Environment Variables

### Core Configuration

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `LLM_PROVIDER` | The LLM provider to use (azure_openai, openai, local_openai) | `azure_openai` |
| `EMBEDDINGS_PROVIDER` | The embeddings provider to use (azure_openai, openai, local_openai) | `azure_openai` |

### Azure OpenAI Configuration

When using `LLM_PROVIDER=azure_openai` or `EMBEDDINGS_PROVIDER=azure_openai`:

| Variable | Description | Required |
|----------|-------------|----------|
| `AZURE_OPENAI_URL` | Azure OpenAI endpoint URL | Yes |
| `AZURE_OPENAI_DEPLOYMENT` | Deployment name | Yes |
| `AZURE_OPENAI_VERSION` | API version | Yes |
| `AZURE_OPENAI_API_KEY` | Azure OpenAI API key | Yes |

### OpenAI Configuration

When using `LLM_PROVIDER=openai` or `EMBEDDINGS_PROVIDER=openai`:

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `OPENAI_API_KEY` | OpenAI API key | Yes | |
| `OPENAI_MODEL` | Model name for LLM | No | `gpt-4` |
| `OPENAI_BASE_URL` | Alternative base URL for OpenAI-compatible endpoints | No | |
| `EMBEDDINGS_MODEL` | Model name for embeddings | No | `text-embedding-3-large` |
| `OPENAI_EMBEDDINGS_BASE_URL` | Alternative base URL for embeddings | No | |

### Local OpenAI Configuration

When using `LLM_PROVIDER=local_openai` or `EMBEDDINGS_PROVIDER=local_openai`:

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `LOCAL_OPENAI_API_KEY` | API key for local service | No | `fake-key` |
| `LOCAL_OPENAI_BASE_URL` | Base URL for local OpenAI service | No | `http://localhost:1234/v1` |
| `LOCAL_OPENAI_MODEL` | Model name for LLM | No | `gpt-4` |
| `LOCAL_EMBEDDINGS_MODEL` | Model name for embeddings | No | `text-embedding-3-large` |
| `LOCAL_OPENAI_EMBEDDINGS_BASE_URL` | Base URL for local embeddings | No | `http://localhost:1234/v1` |

## Usage Examples

### Using Azure OpenAI (Default)

```bash
# .env file
LLM_PROVIDER=azure_openai
EMBEDDINGS_PROVIDER=azure_openai
AZURE_OPENAI_URL=https://your-resource.openai.azure.com/
AZURE_OPENAI_DEPLOYMENT=gpt-4
AZURE_OPENAI_VERSION=2024-08-01-preview
AZURE_OPENAI_API_KEY=your-azure-api-key
```

### Using Standard OpenAI

```bash
# .env file
LLM_PROVIDER=openai
EMBEDDINGS_PROVIDER=openai
OPENAI_API_KEY=your-openai-api-key
OPENAI_MODEL=gpt-4-turbo
EMBEDDINGS_MODEL=text-embedding-3-small
```

### Using Local OpenAI

```bash
# .env file
LLM_PROVIDER=local_openai
EMBEDDINGS_PROVIDER=local_openai
LOCAL_OPENAI_BASE_URL=http://localhost:1234/v1
LOCAL_OPENAI_MODEL=gpt-4
LOCAL_EMBEDDINGS_MODEL=text-embedding-3-large
```

## Backward Compatibility

The system maintains backward compatibility with existing Azure/OpenAI configurations. If you only set `LLM_PROVIDER=azure_openai` and `EMBEDDINGS_PROVIDER=azure_openai`, the system will automatically use the Azure OpenAI configuration as it did before.

## Troubleshooting

1. **Provider Not Found**: Make sure the `LLM_PROVIDER` and `EMBEDDINGS_PROVIDER` variables are set to one of the supported values.
2. **Missing Environment Variables**: Ensure all required environment variables for your selected provider are set.
3. **Connection Issues**: Verify that your API keys and URLs are correct, especially for remote providers.

## Adding New Providers

To add support for new providers, modify the `LLMProvider` and `EmbeddingsProvider` enums in `backend.py` and add corresponding cases in the `create_llm()` and `create_embeddings()` functions.