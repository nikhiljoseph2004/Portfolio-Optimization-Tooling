# Contributing to Portfolio Optimization Tooling

Thank you for your interest in contributing to the Portfolio Optimization Tooling project! This document provides guidelines and instructions for contributing.

## Table of Contents
- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)
- [Submitting Changes](#submitting-changes)

## Code of Conduct

This project adheres to a code of conduct that all contributors are expected to follow:

- Be respectful and inclusive
- Welcome newcomers and help them get started
- Focus on constructive feedback
- Assume good intentions
- Respect differing viewpoints and experiences

## How Can I Contribute?

### Reporting Bugs

If you find a bug, please create an issue with:
- A clear, descriptive title
- Steps to reproduce the issue
- Expected behavior vs. actual behavior
- Your environment (Python version, OS, package versions)
- Any relevant code snippets or error messages

### Suggesting Enhancements

We welcome suggestions for new features or improvements:
- Use a clear, descriptive title
- Provide a detailed description of the proposed feature
- Explain why this enhancement would be useful
- Include examples of how it would work

### Code Contributions

We appreciate code contributions! Here are areas where help is especially valuable:

**High Priority:**
- Additional portfolio optimization algorithms (Black-Litterman, Risk Parity, etc.)
- Improved visualization options
- Performance optimizations for large portfolios
- Better error handling and input validation
- Unit tests and integration tests

**Medium Priority:**
- Additional asset classes (options, futures, crypto)
- Backtesting framework
- Transaction cost modeling
- Tax-loss harvesting strategies
- Multi-period optimization

**Documentation:**
- More examples and use cases
- Video tutorials
- Jupyter notebook walkthroughs
- API documentation

## Development Setup

### Prerequisites
- Python 3.8 or higher
- Git
- pip or conda

### Setting Up Your Development Environment

1. **Fork the repository**
   ```bash
   # Click "Fork" on GitHub, then clone your fork
   git clone https://github.com/YOUR_USERNAME/Portfolio-Optimization-Tooling.git
   cd Portfolio-Optimization-Tooling
   ```

2. **Create a virtual environment**
   ```bash
   # Using venv
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   
   # Or using conda
   conda create -n portfolio-opt python=3.9
   conda activate portfolio-opt
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   
   # Install development dependencies
   pip install pytest black flake8 mypy jupyter
   ```

4. **Create a new branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

## Coding Standards

### Python Style Guide

We follow PEP 8 with some modifications:

- **Line length**: 100 characters (not 79)
- **Imports**: Group stdlib, third-party, and local imports
- **Docstrings**: Use Google-style docstrings
- **Type hints**: Use type hints for function signatures

### Code Formatting

Use `black` for automatic formatting:
```bash
black testing.ipynb  # Format notebooks
black src/           # Format Python modules
```

### Linting

Run `flake8` before committing:
```bash
flake8 src/ --max-line-length=100
```

### Type Checking

Use `mypy` for type checking (optional but recommended):
```bash
mypy src/ --ignore-missing-imports
```

### Example of Good Code Style

```python
def calculate_portfolio_return(
    weights: np.ndarray, 
    returns: pd.DataFrame,
    annualize: bool = True
) -> float:
    """Calculate the expected return of a portfolio.
    
    Args:
        weights: Array of portfolio weights, must sum to 1.0
        returns: DataFrame of historical returns (rows=dates, cols=assets)
        annualize: Whether to annualize the return (default: True)
    
    Returns:
        Expected portfolio return (annualized if specified)
    
    Raises:
        ValueError: If weights don't sum to 1.0 or shapes don't match
    
    Example:
        >>> weights = np.array([0.5, 0.5])
        >>> returns = pd.DataFrame({'A': [0.01, 0.02], 'B': [0.015, 0.018]})
        >>> calculate_portfolio_return(weights, returns)
        0.1386  # Annualized return
    """
    if not np.isclose(weights.sum(), 1.0, rtol=1e-5):
        raise ValueError(f"Weights must sum to 1.0, got {weights.sum()}")
    
    if len(weights) != returns.shape[1]:
        raise ValueError(
            f"Weights length ({len(weights)}) must match "
            f"number of assets ({returns.shape[1]})"
        )
    
    portfolio_return = np.sum(returns.mean() * weights)
    
    if annualize:
        portfolio_return *= 252  # Trading days per year
    
    return float(portfolio_return)
```

## Testing Guidelines

### Writing Tests

All new code should include tests. We use `pytest` for testing.

**Test file structure:**
```
tests/
├── __init__.py
├── test_data_fetching.py
├── test_optimization.py
├── test_statistics.py
└── fixtures/
    └── sample_data.csv
```

**Example test:**
```python
import pytest
import numpy as np
import pandas as pd
from src.portfolio import calculate_portfolio_return

def test_calculate_portfolio_return_basic():
    """Test basic portfolio return calculation."""
    weights = np.array([0.5, 0.5])
    returns = pd.DataFrame({
        'A': [0.01, 0.02, 0.015],
        'B': [0.015, 0.018, 0.020]
    })
    
    result = calculate_portfolio_return(weights, returns, annualize=False)
    expected = (0.015 * 0.5) + (0.0177 * 0.5)  # Approximate
    
    assert np.isclose(result, expected, rtol=1e-2)

def test_calculate_portfolio_return_weights_validation():
    """Test that invalid weights raise ValueError."""
    weights = np.array([0.5, 0.6])  # Sum > 1
    returns = pd.DataFrame({'A': [0.01], 'B': [0.015]})
    
    with pytest.raises(ValueError, match="Weights must sum to 1.0"):
        calculate_portfolio_return(weights, returns)
```

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src --cov-report=html

# Run specific test file
pytest tests/test_optimization.py

# Run specific test
pytest tests/test_optimization.py::test_calculate_portfolio_return_basic
```

### Test Coverage

Aim for at least 80% code coverage for new code. Check coverage with:
```bash
pytest --cov=src --cov-report=term-missing
```

## Documentation

### Docstring Format

Use Google-style docstrings:

```python
def function_name(param1: type, param2: type) -> return_type:
    """Brief description of function.
    
    Longer description if needed. Can span multiple lines
    and include mathematical formulas or detailed explanations.
    
    Args:
        param1: Description of param1
        param2: Description of param2
        
    Returns:
        Description of return value
        
    Raises:
        ExceptionType: Description of when this is raised
        
    Example:
        >>> function_name(value1, value2)
        expected_output
        
    Note:
        Any additional notes or warnings
    """
    pass
```

### Updating Documentation

When adding new features:
1. Update the relevant `.md` files in `docs/`
2. Add examples to `docs/EXAMPLES.md`
3. Update the main `README.md` if needed
4. Add inline comments for complex algorithms

### Jupyter Notebooks

For notebooks:
- Add markdown cells explaining each section
- Include example outputs
- Keep code cells focused and readable
- Add comments for complex operations

## Submitting Changes

### Pull Request Process

1. **Update your branch**
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Run tests and linting**
   ```bash
   pytest
   black .
   flake8 src/
   ```

3. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add feature: brief description"
   ```
   
   Use clear, descriptive commit messages:
   - Use present tense ("Add feature" not "Added feature")
   - Be specific about what changed
   - Reference issues if applicable (#123)

4. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Create a Pull Request**
   - Go to GitHub and create a PR from your branch
   - Fill out the PR template completely
   - Link to any related issues
   - Add screenshots for UI changes

### Pull Request Checklist

Before submitting, ensure:
- [ ] Code follows the style guidelines
- [ ] All tests pass
- [ ] New tests added for new functionality
- [ ] Documentation updated
- [ ] No unnecessary files included
- [ ] Commit messages are clear
- [ ] PR description explains the changes

### PR Review Process

1. Maintainers will review your PR
2. Address any feedback or requested changes
3. Once approved, your PR will be merged
4. Your contribution will be credited

### After Your PR is Merged

1. Delete your branch
   ```bash
   git branch -d feature/your-feature-name
   git push origin --delete feature/your-feature-name
   ```

2. Update your local main branch
   ```bash
   git checkout main
   git pull upstream main
   ```

## Development Guidelines

### Performance Considerations

- Use NumPy/Pandas operations instead of Python loops
- Cache expensive calculations when possible
- Profile code for bottlenecks before optimizing
- Consider memory usage for large portfolios

### Error Handling

- Validate inputs early
- Provide helpful error messages
- Don't catch exceptions silently
- Log warnings for non-fatal issues

### Security

- Never commit API keys or credentials
- Sanitize user inputs
- Be careful with `eval()` or `exec()`
- Review dependencies for vulnerabilities

## Getting Help

If you need help:
- Check existing issues and documentation
- Ask in issue comments
- Reach out to maintainers

## Recognition

Contributors will be:
- Listed in the project README
- Credited in release notes
- Acknowledged in documentation

Thank you for contributing to Portfolio Optimization Tooling! 🎉

---

**Questions?** Open an issue or contact the maintainers.
