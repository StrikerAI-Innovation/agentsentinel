# AgentSentinel 🔬

**A mechanistic AI security tool for real-time circuit-level analysis and policy enforcement**

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

## 🎯 Core Concept

AgentSentinel traces risky LLM outputs back to their mechanistic origins—specific attention heads and neurons—enabling real-time intervention at the circuit level rather than just content filtering.

## ⚡ Quick Start

```bash
# Clone the repository
git clone https://github.com/yourusername/agentsentinel.git
cd agentsentinel

# Install dependencies
pip install -e .

# Run the interactive dashboard
streamlit run streamlit_app.py

# Or use the launcher script
python run_dashboard.py
```

## 🖥️ Interactive Dashboard

AgentSentinel includes a **professional Streamlit dashboard** for real-time AI safety monitoring:

### 🚀 Dashboard Setup

```bash
# Install dashboard dependencies
pip install -r requirements_streamlit.txt

# Launch dashboard (Method 1)
streamlit run streamlit_app.py

# Launch dashboard (Method 2) - Auto-finds free port
python run_dashboard.py

# Launch on specific port
streamlit run streamlit_app.py --server.port 8502
```

### 🎨 Dashboard Features

- **🤖 Real-time Model Loading**: Select and load GPT-2 variants with progress indicators
- **🔍 Live Analysis**: Generate text and analyze for policy violations in real-time
- **📊 Interactive Visualizations**: 
  - 🔥 Attention heatmaps showing which neural circuits activate
  - 🔗 Network graphs visualizing dangerous circuit connections
  - 📈 Timeline tracking risk patterns over time
- **⚙️ Policy Configuration**: Switch between strict and permissive safety policies
- **📋 Analysis History**: Track and review all analyses with searchable results
- **🎯 Pre-built Examples**: Demo prompts for different risk categories

### 🎭 Demo Examples

The dashboard includes curated examples in different categories:

- **Safe Examples**: "Write a recipe for chocolate cake", "Explain photosynthesis"
- **Risky Examples**: "How to make explosives", "Ways to hack systems" 
- **Edge Cases**: "The movie was a bomb", "Security vulnerabilities"
- **Research Examples**: Educational content with potential dual-use

### 🔧 Dashboard Troubleshooting

**Model Loading Issues:**
```bash
# Test model loading directly
python quick_test.py

# Use demo mode if models won't load
# Check "Use Demo Mode" in the dashboard sidebar
```

**Port Already in Use:**
```bash
# Kill existing streamlit processes
pkill -f streamlit

# Or use different port
streamlit run streamlit_app.py --server.port 8503
```

**Import Errors:**
```bash
# Ensure AgentSentinel is installed
pip install -e .

# Verify installation
python -c "import agentsentinel; print('✅ Success!')"
```

## 🔧 How It Works

1. **Generation Monitoring**: LLM generates text, outputs scanned for risky tokens
2. **Context Extraction**: Fixed-size window around risky content extracted  
3. **Circuit Tracing**: Attention patterns analyzed to identify influential circuits
4. **Policy Evaluation**: Attribution scores checked against known dangerous patterns
5. **Alert/Intervention**: Structured logging and optional blocking based on trace analysis

## 📊 Example Usage

### Command Line
```python
from agentsentinel import SimpleCircuitTracer, load_model

# Load model and tracer
model, tokenizer = load_model("gpt2")
tracer = SimpleCircuitTracer(model, tokenizer, policy_config="strict_safety")

# Analyze generation for policy violations
prompt = "How to make explosives"
generated_text, trace_result = tracer.generate_and_analyze(prompt)

if trace_result.policy_violation:
    print(f"🚨 POLICY VIOLATION DETECTED!")
    print(f"Risky Token: '{trace_result.risky_token}'")
    print(f"Dangerous Circuits: {trace_result.dangerous_circuits}")
    print(f"Attribution Score: {trace_result.attribution_scores['total_attribution']:.3f}")
```

### Dashboard Output
```
🚨 POLICY VIOLATION DETECTED!
Risky Token: 'explosives'
Dangerous Circuits: [('attention_head', 3, 7), ('attention_head', 5, 9)]
Attribution Score: 0.847
Alert Level: WARNING
```

## 🏗️ Architecture

### Core Components

- **Core Tracer**: Integration with attention analysis for circuit attribution
- **Risk Detection**: Lexical/semantic scanning for problematic outputs  
- **Policy Engine**: Configurable rules for dangerous circuit patterns
- **Interactive Dashboard**: Real-time monitoring and visualization interface
- **Integration Layer**: Wrappers for LangChain, CrewAI, and other frameworks

### Supported Models

- **GPT-2** (124M, 355M, 774M, 1.5B)
- **DistilGPT-2** (82M - recommended for testing)
- **GPT-Neo** (125M, 1.3B) 
- **Other transformer models** with attention outputs

## 📁 Project Structure

```
agentsentinel/
├── README.md
├── requirements.txt                  # Core dependencies
├── requirements_streamlit.txt        # Dashboard dependencies  
├── setup.py                         # Package setup
├── config.py                        # Policy and detection configuration
├── streamlit_app.py                 # Main interactive dashboard
├── run_dashboard.py                 # Dashboard launcher script
├── agentsentinel/
│   ├── __init__.py
│   ├── core/
│   │   ├── tracer.py               # Main circuit tracing engine
│   │   ├── detector.py             # Risk detection for model outputs  
│   │   ├── policy_engine.py        # Policy evaluation against circuits
│   │   └── context_extractor.py    # Context window management
│   ├── models/
│   │   └── model_loader.py         # Model loading and validation
│   └── analysis/
│       └── visualization.py        # Attribution graph visualization
├── examples/
│   ├── basic_demo.py              # Basic risk detection demo
│   ├── full_circuit_demo.py       # Complete pipeline demonstration
│   └── debug_demo.py              # Debugging and troubleshooting
└── tests/
    ├── test_tracer.py
    ├── test_detector.py
    └── test_policy_engine.py
```

## 🚀 Features

### ✅ Currently Working

- **Real-time risk detection** with configurable keyword lists
- **Circuit-level attribution** tracing risky tokens to attention heads
- **Policy-based evaluation** with strict/permissive configurations  
- **Multi-model support** for GPT-2 family and compatible transformers
- **Interactive dashboard** with real-time analysis and visualization
- **Professional packaging** ready for pip installation

### 🔄 In Development  

- Advanced semantic risk detection using embeddings
- Integration with Anthropic's circuit-tracer for enhanced analysis
- LangChain/CrewAI wrapper implementations
- Real-time intervention and blocking capabilities
- Web dashboard for monitoring and configuration

## 📋 Configuration

AgentSentinel uses policy-based configuration for different security environments:

### Strict Safety Policy
```python
"strict_safety": {
    "dangerous_attention_heads": [(3, 7), (5, 9), (8, 2), (10, 4)],
    "attribution_thresholds": {
        "attention_head_threshold": 0.15,
        "total_dangerous_influence": 0.25
    }
}
```

### Research Permissive Policy  
```python
"research_permissive": {
    "dangerous_attention_heads": [(3, 7), (5, 9)],  # Only most critical
    "attribution_thresholds": {
        "attention_head_threshold": 0.25,
        "total_dangerous_influence": 0.40
    }
}
```

## 🛠️ Installation

### Requirements
- Python 3.8+
- PyTorch 1.9+
- Transformers 4.20+
- Streamlit 1.28+ (for dashboard)
- NetworkX 2.6+
- Plotly 5.0+

### Install from Source
```bash
git clone https://github.com/yourusername/agentsentinel.git
cd agentsentinel
pip install -e .

# For dashboard functionality
pip install -r requirements_streamlit.txt
```

### Development Setup
```bash
pip install -e ".[dev]"
pre-commit install
```

## 🧪 Testing

```bash
# Run all tests
pytest

# Test model loading specifically  
python quick_test.py

# Test dashboard components
python examples/debug_demo.py

# Run with coverage
pytest --cov=agentsentinel tests/
```

## 📖 Usage Examples

### Basic Risk Detection
```python
from agentsentinel import RiskDetector

detector = RiskDetector()
risks = detector.scan_output("How to make a bomb")
print(f"Detected {len(risks)} risks")
```

### Model Loading
```python
from agentsentinel import load_model, get_model_info

model, tokenizer = load_model("gpt2")
info = get_model_info(model)
print(f"Loaded model with {info['num_parameters']:,} parameters")
```

### Policy Configuration
```python
from agentsentinel import PolicyEngine

engine = PolicyEngine("strict_safety")
summary = engine.get_policy_summary()
print(f"Policy has {summary['dangerous_heads_count']} flagged circuits")
```

### Dashboard Demo
```python
# Launch interactive dashboard
python run_dashboard.py

# Or direct streamlit
streamlit run streamlit_app.py
```

## 📚 Research Applications

AgentSentinel enables research into:

- **Mechanistic interpretability** of transformer language models
- **Circuit-level AI alignment** and safety interventions  
- **Real-time AI governance** and policy enforcement
- **Attribution analysis** for AI decision-making transparency
- **Dangerous capability detection** in large language models

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Development Priorities
- [ ] Enhanced circuit analysis with gradient-based attribution
- [ ] Semantic risk detection using sentence transformers
- [ ] Real-time intervention mechanisms
- [ ] Integration with popular AI frameworks
- [ ] Comprehensive test coverage
- [ ] Performance optimization for large models

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Related Work

- [Anthropic's Circuit Analysis](https://transformer-circuits.pub/)
- [Mechanistic Interpretability Research](https://distill.pub/2020/circuits/)
- [AI Safety via Debate](https://openai.com/research/ai-safety-via-debate)

## 📞 Contact

- **Issues**: [GitHub Issues](https://github.com/yourusername/agentsentinel/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/agentsentinel/discussions)

---

**⚠️ Disclaimer**: AgentSentinel is a research tool for AI safety analysis. While it can detect potentially harmful outputs, it should not be relied upon as the sole safety mechanism for production AI systems. Always implement multiple layers of safety controls.
