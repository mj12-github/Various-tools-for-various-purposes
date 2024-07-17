import math
from collections import Counter

# Function to calculate Shannon entropy
def calculate_entropy(input_string):
    # Count the frequency of each character
    frequency_table = Counter(input_string)
    
    # Calculate the entropy
    entropy = 0.0
    length = len(input_string)
    
    for count in frequency_table.values():
        probability = count / length
        entropy -= probability * math.log2(probability)
    
    return entropy

# Function to process the input file and calculate entropy for each line
def process_file(input_file_path, output_file_path):
    try:
        # Open the input file and read all lines
        with open(input_file_path, 'r') as input_file:
            lines = input_file.readlines()
        
        # Open the output file for writing
        with open(output_file_path, 'w') as output_file:
            for line in lines:
                line = line.strip()  # Remove any trailing newline characters
                entropy = calculate_entropy(line)
                output_file.write(f"{line} : {entropy}\n")
        
        print(f"Entropy calculation completed. Results written to {output_file_path}")
    except FileNotFoundError:
        print(f"Input file {input_file_path} does not exist.")

# Example usage
# input_file = "input.txt"  # Specify the path to your input text file
# output_file = "output.txt"  # Specify the path to your output text file
# process_file(input_file, output_file)
