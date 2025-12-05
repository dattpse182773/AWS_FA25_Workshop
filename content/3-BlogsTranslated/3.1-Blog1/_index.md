---
title: "Blog 1"
date: 2025-09-10
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}} -->

# Deploying a High Performance Computing Solution for Accurate Weather and Renewable Energy Production Predictions

The field of meteorology has long relied on computational models to predict weather patterns. The advent of High-Performance Computing (HPC) and machine learning (ML) has made these predictions more accurate and reliable. This post outlines the steps for deploying an HPC cluster for weather forecasting on the Amazon Web Services (AWS) Cloud. It examines how cloud-based solutions provide computational flexibility and scalability, demonstrating how HPC and cloud technologies help Iberdrola’s meteorologists calculate renewable energy production forecasts.

For Iberdrola—responsible for 80.3 TWh of renewable energy production in 2024—accurate weather forecasting is essential. Regulatory requirements implemented in Spain in 2004 mandated renewable energy production forecasting, leading Iberdrola Renovables to create MeteoFlow, an in-house forecasting tool with 20 years of continuous development.

Over time, MeteoFlow has evolved to incorporate advanced meteorological simulation techniques, ML, AI, and big data technologies. With the support of multidisciplinary experts, the system now powers accurate short- and long-term energy production predictions across multiple renewable facilities.

MeteoFlow generates production forecasts for Iberdrola’s global onshore wind farms, offshore wind farms, photovoltaic plants, and hydroelectric facilities—using wind predictions, solar radiation forecasts, and additional meteorological inputs. These forecasts support energy market participation, O&M planning, and risk assessment.

Modern meteorological models solve complex nonlinear partial differential equations such as the Navier–Stokes equations, requiring large-scale computation and high-throughput data access. These constraints make the cloud—with its elastic compute resources and virtually unlimited storage—a natural fit for running MeteoFlow forecasting workloads.

---

## Business Value

MeteoFlow provides significant business value by producing accurate power generation forecasts up to:
- **96 hours ahead (hourly)**
- **10 days ahead (daily)**  
for **450+ wind and photovoltaic plants**.

Accurate forecasts help Iberdrola:
- Improve revenue  
- Reduce penalties from market forecast deviations  
- Increase renewable energy competitiveness  
- Optimize O&M operations  
- Improve risk management

MeteoFlow also offers:
- Weather maps through Iberdrola’s intranet  
- Real-time risk alerts (extreme winds, frost, storms)  
- Flow prediction for hydro plants  
- Wave prediction for offshore energy assets  
- Data visualization and analysis via *MeteoWeb*  

---

## MeteoFlow Architecture on AWS

The system migration to AWS focused on three requirements:

1. **Handling 300+ TB of meteorological data** at scale  
2. **Real-time forecasting**, enabling timely decision-making  
3. **Flexibility and modularity** for better integration and cross-team collaboration  

The architecture uses:

### **Amazon S3**
Stores global and regional meteorological model data (IFS, GFS, ICON-EU, custom internal models).

### **Amazon EC2 HPC Instances**
Such as **hpc7a.96xlarge**:
- 192 physical cores  
- 768 GiB RAM  
- EFA networking up to 300 Gbps  
- 4th-gen AMD EPYC CPUs  
- Optimized for tightly coupled HPC workloads  

### **Amazon EFS**
Elastic shared storage for HPC computation results.

### **Amazon SageMaker**
Provides integrated analytics and ML model development for improved forecast precision.

---

## Migrating 300+ TB of Data to AWS

Iberdrola initially considered AWS Snowball but discovered that their on-premises LAN bottleneck would make it no faster than transferring directly to AWS Ireland over their existing high-speed link.

Instead, they used **AWS DataSync**, which:
- Automates large data transfers
- Provides encryption and integrity validation
- Minimizes operational overhead

High-level migration steps included:
1. Setting up AWS Direct Connect
2. Creating VGW & DX Gateway
3. Configuring Private VIF
4. Routing traffic via Direct Connect
5. Deploying a DataSync agent on-prem
6. Creating source & destination (S3) locations
7. Running DataSync tasks

This successfully migrated all historical MeteoFlow datasets to Amazon S3.

---

## Conclusion

Migrating MeteoFlow to the AWS Cloud marks a major milestone in its long development history. AWS enables Iberdrola to use elastic compute and storage, manage massive datasets, and leverage advanced services like Amazon SageMaker. With this cloud-native foundation, Iberdrola can focus on improving ML-driven forecasting models—ultimately strengthening renewable energy production and market performance.
