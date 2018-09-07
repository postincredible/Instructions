# Description 
This instruction will help you download [European Genome-phenome Archive (EGA)](https://ega-archive.org/) data to a HPC server using IBM Aspera

Things you will need before downloading:

  - EGA account and password 




1. Install Aspera on HPC (in case the HPC did not have one)
    
    1.1 Download Aspera
  
    ```
    wget  -nd https://download.asperasoft.com/download/sw/connect/3.8.0/ibm-aspera-connect-3.8.0.158555-linux-g2.12-64.tar.gz
    ```
    Aspera will download to your current directory on HPC
    
    1.2 Unzip package
    
    ```
    tar -xzf ibm-aspera-connect-3.8.0.158555-linux-g2.12-64.tar.gz
    ```
    
    It will generate a `.sh` file
    
    1.3 Install Aspera
    
    ```
    bash ibm-aspera-connect-3.8.0.158555-linux-g2.12-64.sh
    ```
    
    it will install Aspera to user's home directory, for example, /home/youraccountname/.aspera/connect
    
2. Downloading data from EGA

    - Download a specific file on HPC
    
    2.1 CD to the directory with executable ascp
    
        
        cd /home/yhuan162/.aspera/connect/bin
        
        
    2.2 Download a specific file from EGA, for example, download file "EGAD00012341234/ukb_mfi_chrX_v3.txt.cip" to your desired directory 
        
       
        ./ascp --ignore-host-key -d -QT -L- -l 1000M username@xfer.crg.eu:EGAD00012341234/ukb_mfi_chrX_v3.txt.cip /your/desired/directory
        
        
      after input the password associate with account, it will down the file "ukb_mfi_chrX_v3.txt.cip" the direcotry on HPC server


    - Download all EGA files on HPC

    2.1 CD to the directory you wish to save all EGA files
    
    ```
    cd /your/desired/directory
    export PATH=$PATH:~/.aspera/connect/bin/
    ```
    
    Make sure run the export command to make ascp executable before submit the `.sh` script


    2.2 Create a `download.sh` file in your desired directory with following code 
    
    ```
    #!/bin/bash

    #SBATCH -p shared
    #SBATCH --time 0-04:00:00
    #SBATCH --job-name ega
    #SBATCH --output ega-log-%J.txt
    #SBATCH --nodes 1
    #SBATCH --ntasks-per-node=1

    #inits
    export ASPERA_SCP_PASS="yourpawword";
    username="yourusername"
    destination="`dirname $/p`"
    parallel_downloads=8

    #overwrite default parameters?
    if [ ! -z $1 ]; then parallel_downloads="$1"; fi
    if [ ! -z $2 ]; then destination="$2"; fi


    #get small files in its most convenient way
    ascp --ignore-host-key -E "*.cip" -E "download.sh" -d -QTl 100m ${username}@xfer.crg.eu: ${destination}/

    #get not small files in its most convenient way
    cat ${destination}/dbox_content | xargs -i --max-procs=$parallel_downloads bash -c "mkdir -p $destination/\`dirname {}\`; echo \"Downloading {}, please wait ...\"; ascp --ignore-host-key -k 1 -QTl 100m ${username}@xfer.crg.eu:{} ${destination}/{} >/dev/null 2>&1"


    ```

A folder "$" will be crteated, and all files listed in "dbox_content" will be downloaded here.




