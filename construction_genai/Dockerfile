FROM python:3.10-slim

WORKDIR /app

# Upgrade pip and install wheel for faster builds
RUN pip install --no-cache-dir --upgrade pip setuptools wheel

# Copy requirements and install Python dependencies with optimizations
COPY requirements.txt .
RUN pip install --no-cache-dir --prefer-binary -r requirements.txt

# Copy application files
COPY app.py .
COPY llm_engine.py .
COPY vector_db.py .
COPY prompts.py .
COPY data/ ./data/

# Expose Streamlit port
EXPOSE 8501

# Run Streamlit app
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
