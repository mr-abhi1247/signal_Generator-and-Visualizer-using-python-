# Signal Generator and Visualizer

A small Jupyter Notebook project for generating and plotting five basic continuous-time signals: the unit step, an impulse approximation, the ramp, a sine wave, and a cosine wave.

## Project Contents

- `Untitled.ipynb` contains all imports, signal definitions, and plots.
- `README.md` documents the project and how to run it.
- `PROJECT_DOCUMENTATION.pdf` is the PDF version of this guide.
- `generate_documentation_pdf.py` exports this guide as a PDF.
- `requirements-docs.txt` lists the optional packages used for PDF export.

## Requirements

- Python 3.9 or later
- Jupyter Notebook support (JupyterLab, classic Jupyter, or the VS Code Jupyter extension)
- NumPy
- Matplotlib

## Setup

From the project directory, create and activate a virtual environment on Windows:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install numpy matplotlib jupyter
```

If PowerShell prevents environment activation, either allow activation for the current terminal according to your organization's policy, or use the environment interpreter directly:

```powershell
.\.venv\Scripts\python.exe -m pip install numpy matplotlib jupyter
```

## Run the Notebook

Open `Untitled.ipynb` in VS Code or start JupyterLab from the project directory:

```powershell
python -m jupyter lab
```

Select the Python environment created above as the notebook kernel, then run the cells from top to bottom. The final cell displays five vertically stacked plots. The notebook does not save the figure to disk; use the plotting interface's save control if you want an image file.

## How It Works

The notebook creates a time vector with `numpy.arange(-5, 5, 0.01)`. This produces 1,000 samples from -5 through 4.99 seconds, spaced 0.01 seconds apart. Each signal is calculated at those time points and plotted with Matplotlib.

### Unit Step

```text
u(t) = 0 for t < 0; 1 for t >= 0
```

The implementation uses `numpy.where(t >= 0, 1, 0)`. The plot changes from zero to one at time zero.

### Unit Impulse Approximation

The notebook uses `numpy.isclose(t, 0)` to place a value of one at the sample nearest zero and zero at all other samples. The plot displays every tenth time sample to make the discrete marker easier to see.

This is a discrete visual approximation, not a mathematically exact Dirac delta. In continuous-time signal theory, an impulse has unit area. Here, the single sample has height one and, with a 0.01-second sample interval, area approximately 0.01. For a unit-area discrete approximation, set the selected sample's height to `1 / dt`, where `dt` is the sample interval.

### Ramp

```text
r(t) = max(t, 0)
```

The implementation uses `numpy.maximum(t, 0)`, giving zero for negative time and a linearly increasing value for nonnegative time.

### Sine and Cosine

```text
x_sin(t) = sin(2 pi t)
x_cos(t) = cos(2 pi t)
```

Both waves have a frequency of 1 Hz and a period of 1 second. The sine starts at zero at time zero; the cosine starts at one.

## Changing the Signals

The time range and resolution are set by the time-vector expression. For example, a 0.001-second interval provides finer sampling:

```python
dt = 0.001
t = np.arange(-5, 5, dt)
```

The frequency of the periodic signals can be made configurable:

```python
frequency = 2
sine = np.sin(2 * np.pi * frequency * t)
cosine = np.cos(2 * np.pi * frequency * t)
```

The impulse expression can also use the selected interval to approximate unit area:

```python
impulse = np.where(np.isclose(t, 0), 1 / dt, 0)
```

For reliable sampling of a sine or cosine, use a time step much smaller than half the period (`1 / (2 * frequency)`); substantially finer sampling usually produces smoother plots.

## Plot Layout

Matplotlib creates a 12-by-10-inch figure with five rows and one column. Each row has a title and grid. The impulse uses `plt.stem` on every tenth sample; the other signals use line plots. `plt.tight_layout()` reduces overlap between plot labels, and `plt.show()` displays the result.

## Export the Documentation PDF

The PDF export tools are separate from the notebook's runtime dependencies. Install them and regenerate the PDF from the project directory with:

```powershell
python -m pip install -r requirements-docs.txt
python generate_documentation_pdf.py
```

The script reads this README and writes `PROJECT_DOCUMENTATION.pdf` beside it.

## Troubleshooting

- **`ModuleNotFoundError` for NumPy or Matplotlib:** Install the packages in the same Python environment selected as the notebook kernel: `python -m pip install numpy matplotlib`.
- **The notebook uses the wrong Python environment:** Select the `.venv` interpreter/kernel in VS Code or Jupyter, then restart the kernel and rerun the cells.
- **No plot appears:** Run the cells in order and confirm that the final cell reaches `plt.show()`.
- **The impulse appears faint or absent:** It is nonzero at only one sample. The notebook plots a downsampled set of points, including the zero-time point, using a stem plot.

## Scope and Limitations

This is an educational example, not a reusable signal-processing package or command-line application. It has no configurable interface, automated tests, or image-export routine. The plotted signals are generated directly in the notebook, and the impulse is represented by a sampled marker rather than a continuous-time distribution.

## License

No license is declared in the repository. Ask the project owner before redistributing or reusing the code beyond applicable default copyright rules.
