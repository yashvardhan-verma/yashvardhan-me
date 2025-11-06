+++
title = "Automating Driving Tests with Computer Vision"
date = "2025-03-10"

[taxonomies]
tags = ["computer-vision", "automation", "backend"]
[extra]
repo_view = true
comment = true
+++

# Automating Driving Tests with Computer Vision

The **Automated Driving Test System (ADTS)** was developed for the **Gujarat Motor Vehicle Department** to replace manual driving assessments with real-time, camera-based evaluation.

## Problem
Traditional driving tests depend heavily on manual supervision, which can be inconsistent and time-consuming. The goal was to automate the process using multi-camera setups and backend systems capable of evaluating candidates objectively.

## Approach
We designed a system that integrates:
- **Multi-camera RTSP streams** for real-time visual analysis  
- **Computer vision pipelines** using OpenCV for lane tracking, stop-line detection, and rule violations  
- **Python FastAPI backend** to handle data flow, candidate information, and result computation  
- **PostgreSQL** for persistent data storage and test results

## Results
- Fully automated evaluation with **zero manual intervention**  
- Real-time scoring aligned with official driving parameters  
- Seamless integration with government hardware and dashboards  

The project demonstrated how **AI + backend engineering** can eliminate human bias and improve large-scale public systems.
