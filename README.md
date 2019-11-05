# signals_functions
Ongoing repo to sort of mimic the functionality Matlab signals toolkit.

All the functions are defined in the first cell of the notebook. If someone wants to pull these into a module, be my guest -- open a pull request. I will try to keep this updated after lecture with new, useful functions as Delchamps teaches them.

### Documentation
##### Signal Generating Functions Overview
```python
def unit_step(n)
def impulse(n)
def const(a)
def zero(n)
def power_signal(gamma)
def sinc(arg)
def dirichlet(N,arg)
def rect(duty_cycle=0.5, amp=1, period=2, count=1, dc_offset=0, t_offset=0)
def gen_signal(start, end, signal=zero, bk=[])
def convolve(x1,x2)
```
###### Unit-Step
Named function that returns 1 if the input (i.e. index) is greater than zero, otherwise returns 0. Equivalent to `u[n]`.
###### Impulse
Named function that returns 1 if the input (i.e. index) is equal to zero, otherwise returns 0. Equivalent to the discrete-time delta function.
###### Const
Named function that returns a lambda function, that in turn returns `a` for all input values `n`, i.e. a constant function. Equivalent to `c[n] = a`. Const is defined as such to make use in gen_signal more consistent.
###### Zero
Named function that returns the zero signal, i.e. zero for all `n`. Equivalent to `const(n,0)` and `z[n] = 0`.
###### Power Signal
Named function that returns a lambda (anonymous) function of `n`. Note that this is similar to how `const` works. The lambda function returns the input `gamma` to the power n, i.e. `power_signal(2)(n) = 2**n`. Equivalent to `p[n] = gamma^n`.
###### Sinc
Returns a lambda function that evaluates the sinc function `sin(arg*n)/(arg*n)` in discrete-time.
###### Dirichlet
Returns a lambda function that evaluates the Dirichlet function `sin(N*arg*x)/(N*sin(arg*x))` in discrete-time.
###### Gen Signal
This is the workhorse of this signals workbook. `gen_signal` returns a list containing a lambda function at index 0 and a tuple at index 1. Rather than tying the begin and end indices `n` (taking `n` in the mathematical sense, where `n` can range from negative infinity to positive infinity) via parameters in convolve, DTFT, graph_signal, and print_signal, the begin and end indices are recorded in the tuple. The default lambda function is the zero signal, but start and end must be specified parameters. All following functions expect the output of `gen_signal` as their input. Note that while both `signal` and `bk` can be specified, it is recommended that you only use one or the other for clarity of code.
Usage is as follows:
```python
sig1 = gen_signal(-10, 10, signal = unit_step)
sig2 = gen_signal(-10, 10, signal = power_signal(2))
sig3 = gen_signal(-10, 10, bk = [1,5,10,4,-10])
sig4 = gen_signal(10, 20, signal = const(-3), bk = [2, 4, 6, 8, -2, -3, -4, -5])
```
###### Convolve
This takes two signals (the output of gen_signal) and returns a discrete-time convolution of them. This can have unexpected results, for example when convolving a unit step signal. An open issue is to make convolve work for more general signals. Convolve returns a new `gen_signal` defined only with bk's.

##### Other Functions
```python
def DTFT(signal, num_pts=100)
def change_range(signal, new_start, new_end)
def graph_signal(signal)
def print_signal(signal)
```
###### DTFT
Graphs the magnitude and phase of the DTFT between -pi and pi. The default number of frequency samples is 100. TODO: make DTFT return periodic outputs of gen_signal.
###### Change Range
Changes the internal range of a signal (result of gen_signal). This can have impacts on DTFT, convolve, print_signal, and graph_signal.
###### Graph signal
Plots each index between the start and the end of the signal in a stem plot.
###### Print signal
Prints `n` and `x[n]` for each index in the range of the signal. Slightly more clean than using default list-print options.
