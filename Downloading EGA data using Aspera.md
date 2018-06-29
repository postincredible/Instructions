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

    2.A Download a specific file on HPC
    
        2.A.1 CD to the directory with executable ascp
        
        ```
        cd /home/yhuan162/.aspera/connect/bin
        ```
        
      2.A.2 Download a specific file from EGA, for example, download file "EGAD00010001474/ukb_mfi_chrX_v3.txt.cip" to directory "/projects/ps-janssen3/dsci-pa/yhuan162/ukbb/EGAD/RawData"
        
        ```
        ./ascp --ignore-host-key -d -QT -L- -l 1000M ega-box-851@xfer.crg.eu:EGAD00010001474/ukb_mfi_chrX_v3.txt.cip /projects/ps-janssen3/dsci-pa/yhuan162/ukbb/EGAD/RawData
        ```










