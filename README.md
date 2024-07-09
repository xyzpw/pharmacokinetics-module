# pharmacokinetics
The **pharmacokinetics** package is a Python package designed to make pharmacokinetic formulas easier to calculate in your Python code.

## Usage
Some functions will use kwargs, which will allow the ability to use alternatives to values, e.g. the parameter `t12` can be used instead of `ke`, which will convert the elimination half-life to the elimination rate constant with the following formula:

$\Large{\frac{\ln2}{t^{1/2}}}$

> [!NOTE]
> Remember to make sure your units match!

### Calculating Concentrations
Calculating the concentration remaining after an elapsed time after peak concentration using the formula $C \cdot e^{-k_et}$:
```python
import pharmacokinetics as pk
pk.single_dose.calculateRemaining(initial_concentration=10, time_elapsed=4, t12=9)
```
The above code will calculate the remaining concentration of a drug that has reached peak concentration 4 hours ago with an elimination half-life of 9 hours.

The formula to this function:<br>
$10 \ mg \cdot e^{-\frac{\ln2}{9 \ h}4 \ h}=7.35 \ mg$

To calculate the concentration at any time $T$ (oral administration), the usage is:
```python
import pharmacokinetics as pk
pk.concentrationAtTime(
    dose=200,
    vd=0.7,
    bioavailability=0.99,
    t12=4.5,
    t12abs=7/60,
    time_elapsed=6
)
```
This above code follows the formula:

$\frac{F \cdot D \cdot k_a}{Vd(k_a - k_e)}(e^{-k_e \cdot t} - e^{-k_a \cdot t})$

Alternatively, `dose_interval` can be used if the drug is taken at intervals, this will use the formula:

$\Large{\frac{F \cdot D \cdot k_a}{Vd(k_a - k_e)}(\frac{e^{-k_e \cdot t}}{1 - e^{-k_e \cdot \tau}} - \frac{e^{-k_a \cdot t}}{1 - e^{-k_a \cdot \tau}})}$

### Solving Values
Half-lives can be solved if the initial concentration, remaining concentration, and time elapsed are known:
```python
import pharmacokinetics as pk
pk.single_dose.halflifeFromDoses(
    initial_dose=15,
    remaining_dose=9,
    time_elapsed=9
)
```
Where the time elapsed is the time past since the drug has reached maximum concentration and begins the elimination phase, which will then follow the formula $C = e^{-x \cdot 9 \ h}$ where $x$ is the elimination rate constant. Solving for $x$ becomes $\frac{\ln(\frac{9}{15})}{9} = -k_e$ to get half-life we use $\frac{\ln2}{|-k_e|} = 12.2 \ h$.

### Calculating Peak Time
If a drug's absorption and elimination constants are known, the tmax can be calculated:
```python
import pharmacokinetics as pk
pk.calculateTmax(t12=9, t12abs=0.75)
```
The formula to this calculation: $\frac{1}{k_a - k_e} \ln(\frac{ka}{ke}) = \frac{\ln(\frac{k_a}{k_e})}{k_a - k_e} = T_{max}$, which results in a tmax of 2.93 hours.

## Disclaimers
This package uses real formulas, but that does not mean it is free from errors, for example, bugs and typos can result in inaccurate info.<br>
If any bugs or inaccuracies are seen, open an issue so it can be fixed.

## Developers
If you intend to install the edited package, create a wheel file:
```bash
$ pip3 install setuptools # required to build package (skip if already installed)
$ python3 -m build # builds the package to a wheel file
```
To install this, I recommend creating a virtual environment:
```bash
$ python3 -m venv .venv # creates virtual environment
$ source .venv/bin/activate # activates the virtual environment
```
Now use pip install with the file that was just created.<br>
To deactivate the virtual environment:
```bash
$ deactivate
```
### Contributing
Contributions must not break the code or change formulas.<br>
Contributions that can possibly be accepted:
- fixed typos
- fixed bugs
- new formulas (source required)

