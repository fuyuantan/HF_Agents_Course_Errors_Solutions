# HF_Agents_Course_Errors_Solutions
The solutions of common errors on HuggingFace_Agents_Course's Final Project!

___

Error 1
```
Error in generating model output:
402 Client Error: Payment Required for url: 
https://api-inference.huggingface.co/models/Qwen/Qwen2.5-Coder-32B-Instruct/v1/chat/completions 

You have exceeded your monthly included credits for Inference Providers. Subscribe to PRO to get 20x more monthly 
included credits.
```
Solution 1
```
from smolagents import CodeAgent, DuckDuckGoSearchTool, LiteLLMModel

model = LiteLLMModel(
    model_id="gemini/gemini-2.0-flash-lite", 
    api_key=os.getenv("GEMINI_API_TOKEN")
) 
self.agent = CodeAgent(
    tools=[DuckDuckGoSearchTool(), VisitWebpageTool()], 
    model=model, 
    max_steps=10
)
```
```      
Require.txt # add
litellm==1.65.8
```
___

Error 2
```
Error fetching questions: 429 Client Error: Too Many Requests for url: https://agents-course-unit4-scoring.hf.space/questions 
```
Solution 2
```
# 2. Fetch Questions
print(f"Fetching questions from: {questions_url}")
try:
    response = requests.get(questions_url, timeout=30)
```
___

Error 3


Solution 3
Add time.sleep() to solve.
```
import time

# 3. Run your Agent
results_log = []
answers_payload = []
print(f"Running agent on {len(questions_data)} questions...")
for item in questions_data:
    task_id = item.get("task_id")
    question_text = item.get("question")
    if not task_id or question_text is None:
        print(f"Skipping item with missing task_id or question: {item}")
        continue
    try:
        submitted_answer = agent(question_text)
        answers_payload.append({"task_id": task_id, "submitted_answer": submitted_answer})
        results_log.append({"Task ID": task_id, "Question": question_text, "Submitted Answer": submitted_answer})
        
        # Add delay
        # There are 30 requests per minute, meaning each request is spaced at least 2 seconds apart. Set 3 seconds as a buffer.
        print("Waiting 3 seconds before next request to avoid rate limit...")
        time.sleep(3)
          
    except Exception as e:
          print(f"Error running agent on task {task_id}: {e}")
          results_log.append({"Task ID": task_id, "Question": question_text, "Submitted Answer": f"AGENT ERROR: {e}"})
          
          # Add delay
          time.sleep(2)![image](https://github.com/user-attachments/assets/595e8f61-effd-47cb-8de1-980b551133c8)
```

