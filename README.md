![Mindfeeder 1 Logo](https://mindfeederllc.com/mindfeeder1.png)

# Mindfeeder-1 OpenAI (Instruction Output) Generator

This Python script uses the OpenAI API to generate instruction-output pairs based on given input text. It automates the process of creating a dataset by splitting the text into sections and generating multiple instruction-output pairs for each section. The results are then saved to a JSON file 

* (optionally) - save the input text chunk used to generate the instruction-output pairs to the "input" in the JSON).
* Output JSON files are to be used with [mindfeeder2](https://github.com/altenderfer/mindfeeder2) -> Providing further augmentation of the dataset that even creates new "input" / context with even more instructions and outputs based on the originals. 

View mindfeeder2 here: [altenderfer/mindfeeder2](https://github.com/altenderfer/mindfeeder2)

## Prerequisites

To run the script, you need the following:

- Python 3.6 or higher
- OpenAI API key
- ```openai``` Python package
- ```txt``` file with data split by 3 new lines, 2 new lines, or 1 new line (Used to help signify where "chunks" are split)

You can install the openai package using the following command:

```
pip install openai
```

## Configuration

Before running the script, you need to provide your OpenAI API key. Edit the apikey argument in the main() function call or pass it as a command line argument.

## Usage

To run the script, execute the following command:

```
python mindfeeder1-open-ai-gen.py --input=input.txt --output=output.json --apikey your_api_key --max_words 300 --num_instructions 12 --max_workers 12
```

The script accepts the following command line arguments:

```
--apikey: Your OpenAI API key (default: "your_api_key")
--model: The OpenAI model to use (default: "gpt-3.5-turbo")
--input: The input file containing the text (default: "input.txt")
--max_words: The maximum number of words allowed in a text chunk (default: 300)
--output: The output file to save the generated dataset (default: "output.json")
--num_instructions: The number of instruction-output pairs to generate for each text chunk (default: 12)
--start_index: The starting index of the input text (default: 0)
--max_workers: The maximum number of concurrent workers (default: 3)
--prompt_input: Additional instructions or context for the model (default: "")
--filter: Whether to filter out irrelevant or unclear instructions and outputs (default: True)
--save_input: Whether to save the input text in the output JSON file (default: True)
```

## How It Works

- The script connects to the OpenAI API using the provided API key.
- It reads the input text file and splits it into sections based on the maximum number of words allowed in a text chunk.
- For each text chunk, the script generates the specified number of instruction-output pairs using the OpenAI API.
- The generated instruction-output pairs are then parsed and filtered based on the given criteria.
- The final dataset is saved as a JSON file in the specified output file.
- **Note: the final JSON file will by default include the chunk of text used to generate the inscrution-output pairs as ```"input"``` with the generated pair. If you only wish to save the instruction-output pairs, use ```--save_input false```

The script uses multithreading to improve performance when generating instruction-output pairs for multiple text chunks. Use ```--max_workers 20``` where ```20``` equals the amount of workers/threads.

### Example

Suppose you have the following input text in a file called input.txt:

```
Section 1: Introduction
Lorem ipsum dolor sit amet, consectetur adipiscing elit.

Section 2: Background
Vivamus lacinia odio vitae vestibulum.
```
You can generate a dataset with instruction-output pairs by running the following command:

```
python mindfeeder1-open-ai-gen.py --input=input.txt --output=output.json

```
This will create a JSON file called output.json with the generated instruction-output pairs:
```
[
  {
    "instruction": "Summarize Section 1",
    "input": "Section 1: Introduction\nLorem ipsum dolor sit amet, consectetur adipiscing elit.\n\nSection 2: Background\nVivamus lacinia odio vitae vestibulum.",
    "output": "Section 1 introduces the topic and discusses the importance of lorem ipsum and its role in consectetur adipiscing elit."
  },
  ...
]
```
## Customization

You can customize the script to suit your needs by modifying the parameters in the main() function call or passing them as command line arguments. For example, you can change the number of instruction-output pairs generated for each text chunk, the maximum number of concurrent workers, or whether to save the input text as input in the output JSON file.

## Troubleshooting

If you encounter any issues while running the script, make sure to check the following:

- Ensure that you have a valid OpenAI API key and the openai package installed.
- Double-check the input and output file paths to ensure they are correct.
- Make sure the input text is formatted correctly with appropriate section breaks. The script initally search for 3 line breaks, then 2 line breaks, then 1 to split or "chunk" the input ```txt``` when being sent to OpenAI.
- Check the command line arguments to ensure they are set correctly.

If the problem persists, you can add error handling or print statements to the script to help identify the issue.

## Limitations

The quality of the instruction-output pairs generated depends on the OpenAI model used and the input text provided. The script may not always generate high-quality or relevant instruction-output pairs, especially for complex or ambiguous input text.
The script may not work as expected for very large input files or when generating a large number of instruction-output pairs due to API limitations or performance constraints.
The script is limited by the performance and capabilities of the OpenAI API. Some features or functionalities may not be available or may change over time.

# Frequently Asked Questions (FAQs)

### What is the purpose of this script?

This script is designed to generate instruction and output pairs based on a given input text using OpenAI's GPT-3.5-turbo model. It's useful for creating datasets for AI training and testing purposes, as well as generating question and answer pairs for various applications.

### How can I adjust the number of instruction and output pairs generated?

You can adjust the number of instruction and output pairs generated by modifying the ```--num_instructions``` command line argument. For example, to generate 20 pairs, use the following command:

```
python mindfeeder1-open-ai-gen.py --apikey=your_api_key --input=input.txt --num_instructions=20
```

### Can I use other OpenAI models with this script?

Yes, you can use other OpenAI models by changing the ```--model``` command line argument. However, the script has been optimized for the GPT-3.5-turbo model, and using other models may produce unexpected results. However, all chat completetion models are supposed below:


```--model gpt-3.5-turbo``` (8,192 max tokens)

```--model gpt-4``` (8,192 max tokens)

```--model gpt-4-32k``` (32,768 max tokens)


### Can I process a large input text file?

Yes, the script can process large input text files by splitting the text into smaller chunks based on the ```--max_words``` parameter. Each chunk will be processed separately, and the generated instruction and output pairs will be combined in the output JSON file.

### What does the --filter command line argument do?

The ```--filter``` argument enables or disables the filtering of generated instruction and output pairs based on certain keywords. By default, this is set to true, meaning that pairs containing specified keywords or phrases will be excluded from the output. Set this argument to false if you want to include all generated pairs, regardless of their content.

### How do I include the input text chunk in the output JSON file?

By default, the input text chunk is included in the output JSON file. If you want to exclude the input text chunk, set the ```--save_input``` command line argument to false:

```
python mindfeeder1-open-ai-gen.py --apikey=your_api_key --input=input.txt --save_input=false
```

### How can I improve the quality of the generated instruction and output pairs?

You can try adjusting the ```--prompt_input``` command line argument to provide additional context or guidance for the model. This may help in generating more relevant and accurate instruction and output pairs. See the sample below:

```
python mindfeeder1-open-ai-gen.py --apikey=your_api_key --input=input.txt --prompt_input "Be very detailed with your responses and use bullet points in necessary to organize the outputs."
```




# Contributing

If you have any suggestions, bug reports, or improvements, please feel free to submit a pull request or open an issue on the GitHub repository. We appreciate your contributions and feedback!

# Credits
Developed by Kyle Altenderfer ```altenderfer@gmail.com```



# License

This project is licensed under the [GNU General Public License v3.0](LICENSE).

# Donate

Venmo ```@altenderfer```
