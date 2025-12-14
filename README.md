# Lynis Security Auditing Software

## Project Overview

This project focuses on security auditing and system hardening of UNIX-based operating systems using the Lynis auditing tool and CIS Benchmarks as a reference standard.

The main objective is to identify security weaknesses on freshly installed systems, analyze the audit results, apply selected hardening measures, and evaluate security improvements through a second audit.

The project is implemented in a controlled virtual environment using multiple virtual machines. Security recommendations provided by Lynis are compared with relevant CIS Benchmark guidelines in order to justify and explain applied configuration changes.

## Operating Systems

- Ubuntu Server 22.04 LTS  
- Rocky Linux 9  
- FreeBSD 14  

## Tools and Standards

- VirtualBox – virtualization platform  
- Lynis – security auditing tool  
- CIS Benchmarks – security configuration reference  

## Project Workflow

1. Installation of virtual machines with default system configuration  
2. Initial Lynis audit (baseline analysis)  
3. Review of identified warnings and suggestions  
4. Selection and application of system hardening measures  
5. Second Lynis audit after hardening  
6. Comparison and analysis of results before and after hardening  

## Repository Structure

- `docs/` – theoretical background, methodology, CIS references, and conclusions  
- `implementation/` – setup instructions and hardening steps for each operating system  
- `results/` – audit outputs, findings, and screenshots  
- `presentation/` – final presentation slides  

## Team Members

- Lidija Mudri  
- Paula Narančić  
- Vito Petrinjak  
- Luka Pošta  

## How to Run the Project

This project does not contain executable application code.

To reproduce the results, follow the setup and audit instructions provided in the `implementation/` directory for each operating system.
