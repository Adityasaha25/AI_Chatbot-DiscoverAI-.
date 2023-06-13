# AI_Chatbot-DiscoverAI-.
I developed this chatbot using python, OpenAI API Key. To see the website visit 
import openai
import gradio as gr

openai.api_key = "YOUR API KEY"


creator_name = "Aditya Shankar Saha" 
messages = [
    {"role": "system", "content": f"Hello, I am a discoverai created by {creator_name}. How can I help you?"},
]
def chatbot(input):
    if not input:
        return "Please provide a valid input."
    messages.append({"role": "user", "content": input})
    try:
        response = openai.Completion.create(
            engine="text-davinci-002",
            prompt="\n".join([f"{m['role']}: {m['content']}" for m in messages]),
            temperature=0.7,
            max_tokens=2048,
            n=1,
            stop=None,
        )
        if response.choices:
            reply = response.choices[0].text
            messages.append({"role": "discoverai", "content": reply})
            return reply
        else:
            return "An error occurred with the API response. Please try again later."
    except Exception as e:
        return f"An error occurred: {str(e)} Please try again later."


inputs = gr.inputs.Textbox(lines=7, label="Chat with AI")
outputs = gr.outputs.Textbox(label="Reply")

gr.Interface(fn=chatbot, inputs=inputs, outputs=outputs, title="DiscoverAI",
             description="Ask anything you want",
             theme="compact").launch(share=True)

