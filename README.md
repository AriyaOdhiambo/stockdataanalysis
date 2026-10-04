# Extracting and Visualizing Stock & Revenue Data: Tesla and GameStop

A data science project that extracts historical stock prices with **yfinance**, scrapes quarterly revenue data from web pages with **BeautifulSoup**, and visualizes both with **Matplotlib** as a simple dashboard.

The analysis covers two companies:

| Company | Ticker |
|---------|--------|
| Tesla   | `TSLA` |
| GameStop | `GME` |

---

## Table of Contents

- [Project Overview](#project-overview)
- [Tools and Libraries](#tools-and-libraries)
- [Data Sources](#data-sources)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Notebook Walkthrough](#notebook-walkthrough)
- [Sample Output](#sample-output)
- [Troubleshooting](#troubleshooting)
- [Acknowledgements](#acknowledgements)

---

## Project Overview

Extracting essential data from a dataset and presenting it clearly is a core part of data science. In this project I:

1. Pulled the full historical share price for Tesla and GameStop using the `yfinance` API.
2. Scraped quarterly revenue tables for both companies from HTML pages using `requests`, `BeautifulSoup` and `pandas.read_html`.
3. Cleaned the revenue data (removed `$` and `,` characters, dropped null and empty values).
4. Plotted share price and revenue together for each company using a provided `make_graph` function.

> **Note:** The `make_graph` function limits the displayed stock data to dates up to **2021-06-14** and revenue data up to **2021-04-30**, so the graphs only show data up to mid-2021.

---

## Tools and Libraries

- **Python 3**
- [`yfinance`](https://pypi.org/project/yfinance/): historical market data
- [`requests`](https://pypi.org/project/requests/): downloading web pages
- [`beautifulsoup4`](https://pypi.org/project/beautifulsoup4/): parsing HTML
- [`pandas`](https://pandas.pydata.org/): data manipulation and `read_html`
- [`matplotlib`](https://matplotlib.org/): visualization
- [`nbformat`](https://pypi.org/project/nbformat/): notebook support
- Jupyter Notebook / JupyterLab

---

## Data Sources

| Data | Source |
|------|--------|
| Tesla and GameStop stock prices | Yahoo Finance via `yfinance` |
| Tesla quarterly revenue | [revenue.htm](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm) |
| GameStop quarterly revenue | [stock.html](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html) |

---

## Repository Structure

```
.
├── Revenue_Data_and_Building_a_Dashboard_FIXED.ipynb   # Main notebook
├── README.md                                           # Project documentation
└── images/                                             # (optional) screenshots of outputs
```

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>
```

### 2. (Optional) Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install yfinance beautifulsoup4 pandas requests matplotlib nbformat lxml html5lib jupyter
```

### 4. Launch the notebook

```bash
jupyter notebook Revenue_Data_and_Building_a_Dashboard_FIXED.ipynb
```

Then run the cells **in order from top to bottom**. Later cells depend on variables created in earlier ones.

---

## Notebook Walkthrough

| Section | What it does |
|---------|--------------|
| **Graphing function** | Defines `make_graph(stock_data, revenue_data, stock)`, which draws a two-panel chart: share price on top, revenue below. |
| **Question 1** | Creates a `TSLA` ticker, downloads max history into `tesla_data`, resets the index and shows `head()`. |
| **Question 2** | Downloads the Tesla revenue page, parses it, extracts the **quarterly** revenue table into `tesla_revenue`, cleans it and shows `tail()`. |
| **Question 3** | Does the same as Question 1 for `GME` into `gme_data`. |
| **Question 4** | Does the same as Question 2 for GameStop into `gme_revenue`. |
| **Question 5** | Plots the Tesla graph: `make_graph(tesla_data, tesla_revenue, "Tesla")`. |
| **Question 6** | Plots the GameStop graph: `make_graph(gme_data, gme_revenue, "GameStop")`. |

### Key implementation notes

- **Choosing the right table:** each revenue page has two tables, annual (index 0) and quarterly (index 1). The project uses the quarterly table via `soup.find_all("table")[1]`.
- **`read_html` and `StringIO`:** newer versions of pandas treat a raw HTML string as a file path, so the HTML is wrapped in `io.StringIO` before being passed to `pd.read_html`.
- **Cleaning revenue values:** `$` and `,` are stripped with a regex, then null and empty strings are removed so the column can be converted to numbers for plotting.

---

## Sample Output

Add your screenshots to an `images/` folder and reference them here, for example:

```markdown
![Tesla Graph](images/tesla_graph.png)
![GameStop Graph](images/gme_graph.png)
```

---

## Troubleshooting

| Problem | Likely cause and fix |
|---------|----------------------|
| `FileNotFoundError` when using `pd.read_html` | Pass the HTML through `StringIO`: `pd.read_html(StringIO(str(table)))`. |
| `IndexError` on `find_all("table")[1]` | The page didn't download correctly. Print `html_data[:300]` to check what was returned. |
| `YFRateLimitError` or empty stock DataFrame | Yahoo Finance is throttling requests. Wait a minute and re-run, or restart the kernel. |
| `NameError` | A previous cell wasn't run. Run all cells in order. |
| `FeatureNotFound` from BeautifulSoup | You used `lxml` or `html5lib` without installing it. Run `pip install lxml html5lib`. |

---

## Acknowledgements

- Assignment and starter materials provided by the [IBM Skills Network](https://skills.network).
- Stock data courtesy of Yahoo Finance via the `yfinance` library.
