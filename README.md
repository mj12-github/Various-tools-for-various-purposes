# Function to calculate Shannon entropy
function Get-Entropy {
    param (
        [string]$inputString
    )

    # Create a hashtable to count character frequencies
    $frequencyTable = @{}

    # Calculate the frequency of each character
    foreach ($char in $inputString.ToCharArray()) {
        if ($frequencyTable.ContainsKey($char)) {
            $frequencyTable[$char]++
        } else {
            $frequencyTable[$char] = 1
        }
    }

    # Calculate the entropy
    $entropy = 0.0
    $length = $inputString.Length

    foreach ($key in $frequencyTable.Keys) {
        $frequency = $frequencyTable[$key]
        $probability = $frequency / $length
        $entropy += -$probability * [Math]::Log2($probability)
    }

    return $entropy
}

# Function to process the input file and calculate entropy for each line
function Calculate-FileEntropy {
    param (
        [string]$inputFilePath,
        [string]$outputFilePath
    )

    # Check if input file exists
    if (-Not (Test-Path $inputFilePath)) {
        Write-Output "Input file does not exist."
        return
    }

    # Read all lines from the input file
    $lines = Get-Content -Path $inputFilePath

    # Open the output file for writing
    $outputFile = New-Item -Path $outputFilePath -ItemType File -Force

    foreach ($line in $lines) {
        # Calculate entropy for each line
        $entropy = Get-Entropy -inputString $line
        # Write the line and its entropy to the output file
        Add-Content -Path $outputFilePath -Value ("{0} : {1}" -f $line, $entropy)
    }

    Write-Output "Entropy calculation completed. Results written to $outputFilePath"
}

# Example usage
$inputFile = "input.txt"  # Specify the path to your input text file
$outputFile = "output.txt"  # Specify the path to your output text file
Calculate-FileEntropy -inputFilePath $inputFile -outputFilePath $outputFile
