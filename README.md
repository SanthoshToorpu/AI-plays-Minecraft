# Minecraft Bot Code Generation Model

A specialized LLaMA 3.2 model fine-tuned to generate Mineflayer-compatible JavaScript code for Minecraft bot automation.

## Model Details

- **Developed by:** SanthoshToorpu
- **License:** Apache-2.0
- **Base Model:** unsloth/llama-3.2-3b-instruct-bnb-4bit
- **Hugging Face Model:** [SanthoshToorpu/Llama-3.2-Minecraft](https://huggingface.co/SanthoshToorpu/Llama-3.2-Minecraft)

## Overview

This model generates executable JavaScript code using the [Mineflayer](https://github.com/PrismarineJS/mineflayer) API to create intelligent Minecraft bots. It can produce code for various bot behaviors including mining, building, combat, navigation, and server interaction.

## Data Creation Process

1. **Source Dataset**: [MineDojo](https://minedojo.org/) - A comprehensive dataset containing Minecraft gameplay data, tutorials, and documentation
2. **QA Generation**: MineDojo data was processed through LLaMA Scout model hosted on Groq to generate question-answer pairs focused on Minecraft bot programming scenarios  
3. **Data Refinement**: Generated QA pairs were further processed using ChatComplete by Unstip for quality improvement
4. **Fine-tuning**: The curated dataset was used to fine-tune LLaMA 3.2 3B using [Unsloth](https://github.com/unslothai/unsloth) for 2x faster training

## Installation

```bash
pip install transformers torch accelerate
npm install mineflayer
```

## Usage

```python
from transformers import AutoTokenizer, AutoModelForCausalLM

# Load the model from Hugging Face
tokenizer = AutoTokenizer.from_pretrained("SanthoshToorpu/Llama-3.2-Minecraft")
model = AutoModelForCausalLM.from_pretrained("SanthoshToorpu/Llama-3.2-Minecraft")

# Generate bot code
prompt = "Create a bot that automatically farms wheat"
inputs = tokenizer(prompt, return_tensors="pt")
outputs = model.generate(**inputs, max_length=200, temperature=0.7)
code = tokenizer.decode(outputs[0], skip_special_tokens=True)

print(code)
```

## Capabilities

- **Movement & Navigation**: Pathfinding, exploration, terrain traversal
- **Resource Management**: Mining, crafting, inventory organization  
- **Building & Construction**: Automated structure creation
- **Combat Systems**: PvP and PvE bot behaviors
- **Server Integration**: Chat commands, multiplayer coordination

## Example Output

```javascript
const mineflayer = require('mineflayer');

const bot = mineflayer.createBot({
  host: 'localhost',
  username: 'FarmBot'
});

bot.on('spawn', async () => {
  const wheat = bot.registry.itemsByName.wheat_seeds;
  const farmland = bot.findBlock({
    matching: bot.registry.blocksByName.farmland.id,
    maxDistance: 10
  });
  
  if (farmland && bot.inventory.items().find(item => item.name === 'wheat_seeds')) {
    await bot.equip(wheat, 'hand');
    await bot.placeBlock(farmland, new Vec3(0, 1, 0));
  }
});
```

## Training Details

This model was trained using:
- **Framework**: [Unsloth](https://github.com/unslothai/unsloth) and Hugging Face's TRL library
- **Speed**: 2x faster training compared to standard methods
- **Quantization**: 4-bit BNB for efficient inference
- **Dataset Size**: Curated QA pairs generated from MineDojo

## References

- [Hugging Face Model](https://huggingface.co/SanthoshToorpu/Llama-3.2-Minecraft)
- [MineDojo Dataset](https://minedojo.org/)
- [MineDojo Paper](https://arxiv.org/abs/2206.08853)
- [Mineflayer Documentation](https://github.com/PrismarineJS/mineflayer)
- [Unsloth Framework](https://github.com/unslothai/unsloth)

## License

This project is licensed under the Apache-2.0 License.

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests to improve the model or add new capabilities.

## Disclaimer

This model is intended for educational and research purposes. Please ensure compliance with Minecraft's Terms of Service when deploying bots on servers.
