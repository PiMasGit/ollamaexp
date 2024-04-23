# ollamaexp
Experiment with oolama for local LLMs.

copy this to a google colab
```python
!curl https://ollama.ai/install.sh | sh

# !echo 'debconf debconf/frontend select Noninteractive' | sudo debconf-set-selections
# !sudo apt-get update && sudo apt-get install -y cuda-drivers

!pip install pyngrok
from pyngrok import ngrok
ngrok.set_auth_token('2fVy03uNfzbc4DSVG1sFB6DNU4r_3yBqv4AiLRScJ83a1MacY')

import os
import asyncio

# Set LD_LIBRARY_PATH so the system NVIDIA library 
os.environ.update({'LD_LIBRARY_PATH': '/usr/lib64-nvidia'})
os.environ.update({'OLLAMA_HOST': '0.0.0.0'})

async def run_process(cmd):
  print('>>> starting', *cmd)
  p = await asyncio.subprocess.create_subprocess_exec(
      *cmd,
      stdout=asyncio.subprocess.PIPE,
      stderr=asyncio.subprocess.PIPE,
  )

  async def pipe(lines):
    async for line in lines:
      print(line.strip().decode('utf-8'))

  await asyncio.gather(
      pipe(p.stdout),
      pipe(p.stderr),
  )
from IPython.display import clear_output
clear_output()

await asyncio.gather(
    run_process(['ollama', 'serve']),
    run_process(['ngrok', 'http', '--log', 'stderr', '11434']),
)
```

then in a terminal write
```
export OLLAMA_HOST=(Put_your_ngrok_url_link_here)
ollama run dolphin-mistral
``````

TODO:
- install python environment on mac
- install ollama python library
- create a notebook to play with RAG agents -> check langgraph youtube video with llama3 and RAG agents