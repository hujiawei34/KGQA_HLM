# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Knowledge Graph-based Character Relationship Visualization and Q&A System for the Chinese classic novel "Dream of the Red Chamber" (红楼梦). The system provides:

- Character relationship visualization using Neo4j knowledge graph
- Web-based question answering interface
- Character profile display with images
- Interactive relationship exploration

## Key Components

### Application Entry Point
- **app.py**: Flask web server with REST API endpoints for searching relationships, KGQA, and character profiles

### Knowledge Graph Module (`neo_db/`)
- **config.py**: Neo4j database connection configuration and relationship mapping dictionaries
- **creat_graph.py**: Creates knowledge graph from triplet data in `raw_data/relation.txt`
- **query_graph.py**: Handles graph queries for relationships, KGQA answers, and character profiles

### Natural Language Processing (`KGQA/`)
- **ltp.py**: Chinese text processing using LTP (Language Technology Platform) for segmentation, POS tagging, and NER

### Data Sources
- **raw_data/relation.txt**: Character relationship triplets (person1,person2,relationship,category1,category2)
- **spider/images/**: Character portrait images
- **spider/json/data.json**: Character profile data

### Web Interface (`templates/`, `static/`)
- Multi-page Flask application with ECharts.js for graph visualization
- Bootstrap-based responsive design

## Development Setup

### Prerequisites
1. **Neo4j Database**: Requires Neo4j with JDK 8
2. **LTP Models**: Download from [LTP documentation](http://pyltp.readthedocs.io/zh_CN/latest/api.html#id2)

### Installation
```bash
pip install -r requirement.txt
```

### Configuration Steps
1. **Neo4j Setup**: Modify `neo_db/config.py` with your Neo4j credentials:
   ```python
   graph = Graph("http://localhost:7474", auth=("neo4j", "your_password"))
   ```

2. **LTP Model Setup**: Update `KGQA/ltp.py` with your LTP model directory:
   ```python
   LTP_DATA_DIR = '/path/to/ltp_data_v3.4.0'
   ```

3. **Knowledge Graph Creation**: 
   ```bash
   cd neo_db
   python creat_graph.py
   ```

### Running the Application
```bash
python app.py
```
Access at `http://localhost:5000`

## Architecture Notes

### Data Flow
1. User queries processed through LTP for Chinese NLP
2. Extracted entities matched against Neo4j knowledge graph
3. Results formatted as JSON with ECharts.js visualization data
4. Character images served as base64-encoded data

### Key Relationships
- Character categories mapped in `CA_LIST` dictionary (贾家荣国府, 贾家宁国府, 王家, 史家, 薛家, etc.)
- Relationship synonyms handled in `similar_words` dictionary for query flexibility
- Graph queries support bidirectional relationship exploration

### File Dependencies
- `neo_db/creat_graph.py` requires `raw_data/relation.txt`
- `query_graph.py` depends on `spider/images/*.jpg` and `spider/show_profile.py`
- LTP module requires external model files not included in repository