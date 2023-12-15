# DarkGate Install Script retrieval via DNS TXT Record

Recently, I came across a [tweet](https://twitter.com/Unit42_Intel/status/1732857094167023618) from Unit 42 showcasing a interesting technique utilized by DarkGate in order to retrieve DarkGate install script via DNS TXT Records leading to DarkGate infection on the victim machine in their recent campaign.

I developed a PoC for the technique and uploaded it in the following repo which consists of -
  - PDF with embed link (Name: Bank-Statement-poc.pdf)
  - ZIP Archive (Name: Bank-Statement-20231523-poc.pdf.zip)
      - consists of the LNK (Shortcut) file which retrieves install script via DNS TXT Records (Name: Bank-Statement-20231523-poc.pdf.lnk)

*Disclaimer: The PoC does not contain any malicious code =) - also it is a working PoC so you can just use it easily*

### Infection Chain: Working

Following in the infection chain similar to the one seen in the ITW DarkGate campaign - 

At first we open the PDF (Bank-Statement-poc.pdf) which consists of an embedded link which when clicked downloads the ZIP Archive (Bank-Statement-20231523-poc.pdf.zip)

![1](https://github.com/knight0x07/DarkGate-Install-Script-via-DNS-TXT-Record/assets/60843949/c54b58ae-29b0-456d-b5d1-66b36fb60ef7)

The ZIP Archive consists of a LNK (Shortcut) file which once executed runs a command which uses nslookup to retrieve the TXT records from my testing domain "pocdomain[.]linkpc[.]net". **Further I customized the LNK command a bit as the one in the ITW sample was unstable**. The command then parses & dequotes the retrieved TXT record and saves it in the TEMP Directory as "poc.cmd" and then further executes it.

![3](https://github.com/knight0x07/DarkGate-Install-Script-via-DNS-TXT-Record/assets/60843949/12faa83f-2da2-4553-8d3b-0a0b53b4d994)

Here in the below screenshot you can see the retrieved TXT record using nslookup for pocdomain[.]linkpc[.]net. As seen the TXT records consists of a command ;)

![5](https://github.com/knight0x07/DarkGate-Install-Script-via-DNS-TXT-Record/assets/60843949/5c054500-7d77-461e-b94b-e57d67f72725)

Now as explained previously the commands in the TXT record response are parsed, dequoted and then saved as "poc.cmd" in the TEMP Directory and further executed - which in turn would execute calc.exe | waits for 5 seconds | kills the calc process and deletes the "poc.cmd"

![7](https://github.com/knight0x07/DarkGate-Install-Script-via-DNS-TXT-Record/assets/60843949/65f6837f-0cc4-4804-9be4-0b24d5b01e44)

The complete process tree along with the commands can be seen below:

![6](https://github.com/knight0x07/DarkGate-Install-Script-via-DNS-TXT-Record/assets/60843949/9b563ad3-b3f2-41d3-83d8-6d0d53f2e5e6)

Also the retrieved TXT record can be seen:

![10](https://github.com/knight0x07/DarkGate-Install-Script-via-DNS-TXT-Record/assets/60843949/8e66475e-18a8-4245-93fa-fc3cb87f76ed)

In the case of DarkGate ITW sample the TXT records retrieved by the malicious LNK file from the domain consists of commands to download copy of autoit3 and malicious .au3 file further leading to DarkGate infection as shown below

![unit42](https://github.com/knight0x07/DarkGate-Install-Script-via-DNS-TXT-Record/assets/60843949/de3270f4-7b6e-4f36-af54-012fa3f33f00)

Image credits - Unit42_Intel

Thanks,
knight0x07






