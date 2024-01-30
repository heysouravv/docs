---
layout: post
title: "Fast Loading 100 Million Rows into MySQL"
categories: aws
---

I was tasked with importing 100 million rows from a text file into MySQL, the process from data pre-processing to final load revealed impressive statistics and insights. Here's a breakdown of the process and the efficiency gains achieved.

## Data Decompression and Conversion
- Initial Data Size: 32 GB, comprising over 100 million chess game records.
- Decompression Tool: Z standard command line, efficiently unpacking the massive dataset.
- Conversion to CSV: Utilizing Python and the PGN to data module to transform PGN (Portable Game Notation) into CSV format, making it suitable for MySQL import. Despite Python's ease of use, the initial processing speed was not optimal for the dataset's volume.

## Switching to C for Processing
- Processing Speed: Transitioning to C for data processing, with compiler optimizations, ramped up the processing to 5000 games in less than a second. This strategic shift underscored the importance of selecting the right programming language for handling big data efficiently.

## MySQL Data Loading Techniques
- Database Preparation: A MySQL database was set up specifically to accommodate the massive dataset, showcasing the need for careful planning in database architecture.
- Insert Statement Experiment: Initially attempted to load data using Python scripts and insert statements, providing a baseline for performance evaluation.
- Efficiency Gain with load data infile: The load data infile command emerged as a game-changer, executing in half a second and significantly outperforming the insert method. This approach not only increased speed but also enhanced data loading efficiency into MySQL columns.

## Cloud Integration and Final Stats
- Cloud-Hosted Database Preparation: Post-loading, the data was prepared for transfer to a PlanetScale database hosted in the cloud, highlighting the scalability and flexibility of MySQL databases in cloud environments.
- Total Rows Loaded: 100,000,000 rows successfully imported into MySQL, ready for cloud integration, marking a significant achievement in data management and database architecture.

## Key Takeaways
- Language and Tool Selection: The choice of programming language (switching from Python to C) and data import method (load data infile) were pivotal in achieving efficiency.
- Process Optimization: The journey from data conversion to final loading illustrates the critical importance of optimizing each step in the process to handle large-scale datasets effectively.
- Future Readiness: Preparing the dataset for cloud integration showcases the forward-thinking approach necessary for modern data architecture, ensuring scalability and accessibility.

This post not only demonstrates the technical feasibility of loading vast amounts of data into MySQL but also serves as a benchmark for efficiency and speed in database management.