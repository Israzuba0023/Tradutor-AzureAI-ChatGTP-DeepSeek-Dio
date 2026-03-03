# Tradutor-AzureAI-ChatGTP-DeepSeek-Dio



## Overview

This project provides a translation service using various Large Language Models (LLMs) such as OpenAI, DeepSeek, and Azure OpenAI. It allows users to translate text from one language to another by leveraging the APIs of these services. The project demonstrates how to set up and use multiple AI clients for a common natural language processing task.

## Features

*   **Multi-API Support**: Translate text using OpenAI, DeepSeek, and Azure OpenAI.
*   **API Key Management**: Securely manage API keys using Colab's `userdata`.
*   **Unified Translation Function**: A single function to handle translations across different APIs.
*   **Quota/Status Check**: Utility to check the connectivity and basic functionality of the configured APIs.

## Setup

To run this notebook, you will need API keys for the services you wish to use (OpenAI, DeepSeek, Azure OpenAI).

1.  **OpenAI API Key**: 
    *   Go to [OpenAI Platform](https://platform.openai.com/account/api-keys).
    *   Create a new secret key.
    *   In Google Colab, go to the left sidebar, click on the "🔑 Secrets" icon.
    *   Add a new secret named `Tradutor` and paste your OpenAI API key as its value.

2.  **DeepSeek API Key**: 
    *   Go to [DeepSeek Platform](https://platform.deepseek.com/api_keys).
    *   Create a new secret key.
    *   In Google Colab, add a new secret named `Tradutor2` and paste your DeepSeek API key as its value.

3.  **Azure OpenAI API Key and Endpoint**: 
    *   You will need an Azure subscription and an Azure OpenAI Service resource.
    *   Obtain your API Key and Endpoint from your Azure OpenAI resource.
    *   In Google Colab, add a new secret named `AZURE_OPENAI_KEY` for the API key.
    *   Add another secret named `AZURE_OPENAI_ENDPOINT` for the Azure OpenAI endpoint (e.g., `https://your-resource-name.openai.azure.com/`).
    *   You will also need to provide the deployment name for Azure OpenAI models. Add a secret named `AZURE_OPENAI_DEPLOYMENT` with the name of your deployed model (e.g., `gpt-35-turbo`).

## Usage

1.  **Run the Initialization Cell**: Execute the first code cell to initialize the API clients.
2.  **Define Text and Target Language**: Set the `article_content` variable to the text you want to translate and `target_language` to the desired language.
3.  **Translate**: Call the `translate_text_with_api` function with the text, target language, and the desired client (e.g., `openai_client`, `deepseek_client`, `azure_client`).

Example:

```python
article_content = "Hello, how are you? I hope you are having a great day."
target_language = "Portuguese"

# Translate using OpenAI
openai_translated = translate_text_with_api(article_content, target_language, openai_client)
print(f"Translated (OpenAI): {openai_translated}")

# Translate using DeepSeek
deepseek_translated = translate_text_with_api(article_content, target_language, deepseek_client)
print(f"Translated (DeepSeek): {deepseek_translated}")

# Translate using Azure OpenAI
azure_translated = translate_text_with_api(article_content, target_language, azure_client)
print(f"Translated (Azure): {azure_translated}")

    Check API Status: Run the check_quotas() function to verify if your API keys are valid and services are reachable.

Dependencies

This project primarily uses the following Python libraries:

    openai: For interacting with OpenAI and DeepSeek APIs.
    azure-openai: For interacting with Azure OpenAI Service.
    google.colab: For secure secret management.

These will be installed automatically in the Colab environment.
Author

[Your Name/Organization Here]
License

[Specify your license here, e.g., MIT License]


[ ]

def translate_text_with_api(text, target_language, api_client):
    try:
        if api_client == openai_client:
            model_to_use = "gpt-3.5-turbo"
        elif api_client == deepseek_client:
            model_to_use = "deepseek-chat"
        elif isinstance(api_client, AzureOpenAI):
            # Azure uses deployment name instead of model name
            model_to_use = userdata.get('AZURE_OPENAI_DEPLOYMENT')
        else:
            return "An error occurred: Invalid API client provided."

        response = api_client.chat.completions.create(
            model=model_to_use,
            messages=[
                {"role": "system", "content": f"You are a highly accurate translation assistant. Translate the following text into {target_language}."},
                {"role": "user", "content": text}
            ],
            temperature=0.7,
            max_tokens=200
        )
        return response.choices[0].message.content.strip()
    except Exception as e:
        return f"An error occurred during translation: {e}"

print("Translation function updated to support Azure, OpenAI, and DeepSeek.")

[ ]

article_content = "Olá, como você está? Espero que esteja tendo um ótimo dia."
target_language = "English"

# Testando com Azure agora
if article_content and target_language:
    print("--- Tentando Tradução com Azure ---")
    azure_translated = translate_text_with_api(article_content, target_language, azure_client)
    print(f"Original text: {article_content}")
    print(f"Translated (Azure): {azure_translated}")
else:
    print("Please ensure 'article_content' and 'target_language' variables are set.")

[ ]

article_content = "Olá, como você está? Espero que esteja tendo um ótimo dia."
target_language = "English"

if article_content and target_language:
    deepseek_translated_text = translate_text_with_api(article_content, target_language, deepseek_client)
    print(f"Original text:\n{article_content}\n")
    print(f"Translated to {target_language} (DeepSeek):\n{deepseek_translated_text}")
else:
    print("Please ensure 'article_content' and 'target_language' variables are set.")

[ ]

def check_quotas():
    print('--- Checking OpenAI Status ---')
    try:
        openai_client.models.list()
        print('OpenAI: API Key is valid and reachable.')
        # Attempt a tiny completion
        openai_client.chat.completions.create(
            model='gpt-3.5-turbo',
            messages=[{'role': 'user', 'content': 'hi'}],
            max_tokens=1
        )
        print('OpenAI: Success (Quota available).')
    except Exception as e:
        print(f'OpenAI Error: {e}')

    print('\n--- Checking DeepSeek Status ---')
    try:
        # DeepSeek usually uses the same OpenAI-compatible list models call
        deepseek_client.models.list()
        print('DeepSeek: API Key is valid and reachable.')
        deepseek_client.chat.completions.create(
            model='deepseek-chat',
            messages=[{'role': 'user', 'content': 'hi'}],
            max_tokens=1
        )
        print('DeepSeek: Success (Balance available).')
    except Exception as e:
        print(f'DeepSeek Error: {e}')

check_quotas()

Produtos pagos do Colab - Cancele os contratos aqui
no final do projeco coloque um readme completo sobre oprojeto

Certainly! I will generate a markdown cell containing a comprehensive README template for your project. You can fill in the specific details.
0 / 100000
Gemini 2.5 Flash
O Gemini pode cometer erros, por isso verifique as respostas e use o código com cuidado. Saiba mais
