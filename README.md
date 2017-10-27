# Enterprise Linux Lab Report - Troubleshooting

- Student name: NAME
- Class/group: TIN-TI-3B (Gent), TIN2-TI-3B (Aalst), TIN-TILE (Afstandsleren) [weglaten wat niet past]

## Instructions

- Write a detailed report in the "Report" section below (in Dutch or English)
- Use correct Markdown! Use fenced code blocks for commands and their output, terminal transcripts, ...
- The different phases in the bottom-up troubleshooting process are described in their own subsections (heading of level 3, i.e. starting with `###`) with the name of the phase as title.
- Every step is described in detail:
    - describe what is being tested
    - give the command, including options and arguments, needed to execute the test, or the absolute path to the configuration file to be verified
    - give the expected output of the command or content of the configuration file (only the relevant parts are sufficient!)
    - if the actual output is different from the one expected, explain the cause and describe how you fixed this by giving the exact commands or necessary changes to configuration files
- In the section "End result", describe the final state of the service:
    - copy/paste a transcript of running the acceptance tests
    - describe the result of accessing the service from the host system
    - describe any error messages that still remain

## Report

### Phase 1: Link layer
- Netwerkkabel aangesloten?
- commando: settings, netwerk, adaptor2, advanced
	- adaptor 1 Nat: ok
	- adaptor 2 Host-only: Nok
- oplossing? 
	- aanvinken Kabel aangesloten
- check?
	- works

### Phase 2: Network layer
- IP adressering juist?
- commando: ip a
	- loopback: ok
	- Nat: ok
	- adaptor 192.168.56.42/24: ok
- check?
	- works
	
### Phase 3: Transport layer
- Draait de service?
- commando: sudo systemctl status nginx.service
	- Active: inactive (dead): Nok
- Oplossing?
	- sudo systemctl start nginx.service
	- sudo systemctl enable nginx.service
	
- check?
	- failed to start the nginx http
- Oplossing?
	- checking configfiles/ errorlogs
	- commando: sudo nginx -t
		- "etc/pki/tls/certs/nigxn.pem" failed
	- typfout aanpassen config file
- check?
	- sudo systemctl status nginx.service
	- Active: running: Ok

- Port?
- commando: sudo ss -tlnp
	- ports: 80, 8443 nginx: Ok

- Firewall permission?
- commando: sudo firewall-cmd --list-all
	- services: dhcpv6-client http ssh: ok
	
	
### Phase 4: Application layer
- Krijg je een pagina in de browser?
- commando: in browser 192.168.56.42
	- No permission
- Oplossing?
	- checking filepermission on index.html
	- commando: ls -l
		- "-rw-r--r--."
...

## End result



## Resources

List all sources of useful information that you encountered while completing this assignment: books, manuals, HOWTO's, blog posts, etc.
