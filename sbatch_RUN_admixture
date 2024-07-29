#!/bin/bash
#SBATCH --job-name=admixturerun25    # Job name
#SBATCH --partition=large            # Partition name
#SBATCH --nodes=1                    # Use only one node
#SBATCH --ntasks=1                   # Run a single task
#SBATCH --cpus-per-task=84           # Number of CPU cores per task
#SBATCH --mem=128gb                  # Total memory per node
#SBATCH --time=5-20:00:00            # Time limit days-hrs:min:sec
#SBATCH --output=admixture_25run_%j.log  # Standard output and error log
#SBATCH --mail-type=ALL              # Mail events (BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=nnnn@gmail.com  # Where to send mail

# Print date, hostname, and current directory
date
hostname
pwd

# Define the number of bootstrap iterations
BOOTSTRAP_ITERATIONS=10

# Define the source file
SOURCE_FILE="example.ped"

# Loop for the number of bootstrap iterations
for i in $(seq 1 $BOOTSTRAP_ITERATIONS); do
  # Create a directory for the current iteration
  RUN_DIR="run_${i}"
  mkdir -p ${RUN_DIR}

  # Copy the source file into the current iteration directory
  cp ${SOURCE_FILE} ${RUN_DIR}

  # Change to the current iteration directory
  cd ${RUN_DIR}

  # Define a unique seed for this iteration
  SEED=$((12345 + i))

  # Loop through K values from 3 to 15
  for K in {3..15}; do
    # Create a unique output file for each K value in the current iteration
    OUTPUT_FILE="cv_results_${K}.out"

    # Run admixture with cross-validation and append results to the output file
    admixture -s ${SEED} --cv=20 -j84 ${SOURCE_FILE} $K | tee -a ${OUTPUT_FILE}
  done

  # Change back to the parent directory
  cd ..
done
