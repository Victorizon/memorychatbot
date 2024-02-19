# memorychatbot

from openai import OpenAI
import tiktoken
openai.api_key = 'x'
client = OpenAI(api_key=api_key)
from IPython.display import display, Markdown

#Print all available models
#from datetime import datetime
#sorted_models = sorted(models_response.data, key=lambda model: model.created)
#for model in sorted_models:
#    readable_date = datetime.utcfromtimestamp(model.created).strftime('%Y-%m-%d %H:%M:%S')
#    print(f"Model ID: {model.id}, Created: {readable_date}, Owned by: {model.owned_by}")


def chat_with_gpt(model_name="gpt-3.5-turbo-0125"):
    messages = []
    while True:
        user_message = input("[You]: ")
        user_tokens = calculate_tokens(user_message)
        messages.append({"role": "user", "content": user_message})
        completion = openai.chat.completions.create(
            model=model_name,
            messages=messages
        )
        print("\n")
        model_response = completion.choices[0].message.content
        print("[GPT]:", model_response)
        completion_tokens = dict(completion).get('usage').completion_tokens
        prompt_tokens = dict(completion).get('usage').prompt_tokens
        total_cost = prompt_tokens*0.0000005 + completion_tokens*0.0000015
        print('\n' + '█')
        print(f" >>>>> {dict(completion).get('usage')} / Total Cost: ${total_cost:.10f} <<<<< ")
        print('_' * 125 + '\n')
        
        messages.append({"role": "assistant", "content": model_response})
        if user_message.lower() == "stop":
            print("Ending the chat.")
            break
chat_with_gpt()
