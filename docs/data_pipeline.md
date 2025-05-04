# Data Processing Pipeline (数据处理流程)

To power the AI features and provide accurate information, raw conference data needs to be processed and loaded into the system's knowledge base. This document outlines the conceptual data pipeline.

为了驱动AI功能并提供准确信息，原始的会议数据需要被处理并加载到系统的知识库中。本文档概述了概念性的数据处理流程。

**(A simple flowchart diagram illustrating the steps below would be beneficial here.)**
*（此处若配一个简单的流程图将更有助于说明以下步骤。）*

## 1. Data Sources (数据源)

The pipeline ingests information from various sources, typically provided by the conference organizers:

数据流程从多个来源获取信息，通常由会议组织者提供：

*   **Attendee Lists:** Spreadsheets or database exports containing names, emails, job titles, departments, registration status, VIP status, special requirements (dietary, accessibility). (参会者名单：包含姓名、邮箱、职位、部门、注册状态、VIP身份、特殊需求等的表格或数据库导出文件。)
*   **Agenda/Schedule:** Documents (Word, PDF, Excel) or structured data detailing sessions, times, locations, speakers, descriptions. (议程/日程表：详细说明会议、时间、地点、演讲者、描述的文档或结构化数据。)
*   **Speaker Information:** Bios, photos, presentation titles, abstracts. (演讲者信息：简介、照片、演讲题目、摘要。)
*   **Venue Information:** Maps (floor plans), room names, facility locations (restrooms, dining areas), address, transport links. (场地信息：地图（楼层平面图）、房间名称、设施位置、地址、交通链接。)
*   **Presentation Content:** Speaker slides (PPT, PDF) or report documents (Word, PDF) provided pre-conference or during. (演示内容：会前或会中提供的演讲者幻灯片或报告文档。)
*   **Pre-defined FAQs:** A list of frequently asked questions and their official answers. (预定义常见问题解答：常见问题及其官方答案列表。)

## 2. Data Extraction & Transformation (数据提取与转换)

Raw data is processed to extract meaningful information:

原始数据经过处理以提取有效信息：

*   **Text Extraction:** Extracting text content from various file formats (PDF, DOCX, PPTX). May involve Optical Character Recognition (OCR) for image-based PDFs or slides. (从各种文件格式中提取文本内容。对于基于图像的PDF或幻灯片可能需要OCR。)
*   **Data Cleaning:** Standardizing formats (e.g., dates, times), correcting typos, handling missing values. (标准化格式，纠正错别字，处理缺失值。)
*   **Entity Recognition:** Identifying key entities like names, locations, times, organizations within unstructured text. (在非结构化文本中识别关键实体，如人名、地名、时间、组织。)
*   **Structuring:** Mapping extracted information to the predefined schemas of the relational database (e.g., creating records for sessions, speakers, attendees). (将提取的信息映射到关系数据库的预定义模式，如创建会议、演讲者、参会者记录。)

## 3. Knowledge Base Population (知识库填充)

The processed data is loaded into the appropriate storage systems:

处理后的数据被加载到相应的存储系统中：

*   **SQL Database:** Structured data (attendee profiles, final schedule, speaker details, room assignments) is inserted or updated in the relational database (e.g., PostgreSQL). This data allows for precise lookups and filtering. (结构化数据被插入或更新到关系数据库。这些数据支持精确查找和过滤。)
*   **Vector Database:**
    *   **Chunking:** Long documents (reports, transcripts) or relevant text sections (speaker bios, session descriptions, FAQs) are broken down into smaller, semantically meaningful chunks.
    *   **Embedding:** Each chunk is passed through a text embedding model (e.g., M3E, BGE, or TianSuan's model) to generate a high-dimensional vector representation.
    *   **Indexing:** These vectors, along with their corresponding text chunks and metadata (source document, section title), are stored and indexed in the vector database (e.g., Milvus). This enables fast similarity searches for the RAG system. (长文档被分割成小块，每块通过文本嵌入模型生成向量，存入向量数据库并建立索引。这使得RAG系统能够快速进行相似性搜索。)

## 4. Knowledge Base Updates (知识库更新)

The pipeline must handle updates:

数据流程必须能处理更新：

*   **Real-time Changes:** Mechanisms are needed to update the knowledge base quickly if there are last-minute changes (e.g., room change, schedule adjustment). This could involve manual updates via an admin interface or automated triggers. (需要机制来快速更新知识库以反映临时变更。可通过管理界面手动更新或自动触发。)
*   **Incremental Processing:** During the conference, new data (like session transcripts or feedback) is generated and needs to be processed and added to the knowledge base. (会议期间产生的新数据需要被处理并添加到知识库中。)

This data pipeline ensures that the AI Conference Assistant operates on accurate, comprehensive, and up-to-date information, forming the foundation for its intelligent capabilities.
该数据流程确保AI会议助手基于准确、全面、最新的信息运行，为其智能能力奠定基础。
