---
title: "Blog 3"
date: 2025-09-10
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}} -->

# How DTN accelerates operational weather prediction using NVIDIA Earth-2 on AWS

Accurate and timely weather forecasting is becoming increasingly essential for industries such as energy, agriculture, aviation, and maritime operations. Traditional physics-based numerical weather prediction (NWP) models require significant compute resources and time, making it challenging to scale during high-impact weather events. To address this, DTN collaborated with NVIDIA and AWS to integrate **NVIDIA Earth-2**, an AI-driven weather modeling platform, into their production system—enabling faster, more scalable, and highly accurate weather predictions.

This blog explains how DTN built a cloud-native, GPU-accelerated forecasting workflow on AWS using **Earth2Studio**, **AWS Batch**, **EC2 GPU instances**, and **Step Functions**. It also outlines how the system improves cyclone tracking accuracy, reduces operational risk, and supports mission-critical decision-making for DTN’s global customers.

---

## Overview of NVIDIA Earth-2

**NVIDIA Earth-2** is an enterprise-grade platform designed for developing and running AI-powered weather and climate models. It provides:

- Accelerated pipelines for training physics-informed AI models  
- Pre-trained weather models such as *FourCastNet*  
- High-resolution super-resolution models like *CorrDiff*  
- Visualization and simulation workflows  
- Integration tools for real-time data from sources like the Registry of Open Data on AWS  

AI-based inference dramatically reduces forecast runtime—from hours to seconds—allowing organizations to generate large ensemble predictions quickly and cost-effectively.

---

## DTN’s AI Weather Forecasting Pipeline on AWS

DTN implemented a fully managed, scalable inference pipeline using Earth2Studio and AWS cloud services. A typical forecast workflow includes:

1. **Data Preparation**  
   - AWS Lambda prepares input weather conditions  
   - Python utilities format the data for ingestion  

2. **AI Model Inference**  
   - AWS Batch deploys Earth2Studio workloads on EC2 GPU instances (G6e, P5, P6)  
   - FourCastNet generates ensemble forecasts at 25-km global resolution within seconds  

3. **Post-Processing & Aggregation**  
   - A Python container computes ensemble statistics  
   - Forecast uncertainty is quantified using measures such as standard deviation  

> This architecture enables DTN to scale forecast generation on demand—especially useful for cyclone seasons or extreme weather events.

---

## Cloud-Native Architecture

DTN’s production system is built using containerized workflows and event-driven orchestration.

### Key AWS components:

- **Amazon ECR** – stores GPU-optimized inference containers  
- **AWS Batch** – schedules and manages GPU/CPU workloads  
- **AWS Step Functions** – orchestrates multi-stage forecasting pipelines  
- **Amazon S3** – central data lake for inputs, outputs, and model artifacts  
- **Amazon SNS** – triggers notifications for downstream processes  
- **s3fs-mounted S3 volumes** – accelerate access to large meteorological datasets  

To ensure reliability, the solution integrates:

- Step Functions retry logic  
- Dead-letter queues for fault isolation  
- Cached input data to reduce repeated downloads  
- Full infrastructure-as-code deployment via **AWS CDK**

---

## Validation and Performance

DTN validated the solution using real historical events such as Hurricanes Milton, Helene, and Lee. Ensemble forecasts produced by the Earth-2 models showed strong agreement with observed “BestTrack” data, illustrating the model’s ability to generate accurate cyclone tracks rapidly and consistently.

The system went live in June 2025 after a four-month development cycle, leveraging AWS and NVIDIA expertise to harden the workflow for production reliability.

---

## Business Impact

By integrating AI-driven forecasts into its operational systems, DTN provides:

- More accurate and timely weather predictions  
- Better risk assessment for high-impact weather  
- Improved operational margins for customers  
- Enhanced decision-support dashboards using DTN’s “Decision-Grade Data”  

This allows clients across energy, agriculture, logistics, and supply chain industries to prepare, respond, and plan with greater confidence.

---

## Future Roadmap

DTN plans to continue expanding its AI forecasting capabilities, including:

- Longer forecasting horizons (from hourly to sub-seasonal)  
- More advanced ensemble confidence metrics  
- Additional AI models supported by the Earth-2 platform  
- Integration with broader climate intelligence systems  

The flexibility of NVIDIA Earth-2 and AWS cloud services positions DTN to remain at the forefront of AI-driven weather innovation.

---
