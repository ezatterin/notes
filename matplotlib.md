# General Matplotlib notes

Config directory: `~/.config/matplolib`
Inside it: `./stylelib/` put here `.mplstyle` files. Can see available styles with:

```python
print(plt.style.available)
```

And use one:

```python
plt.style.use('edotex')
```

Also, can tweak the matplolibrc file, in the config dir.

One of the many ways to make your plots look nice with latex:

```python
latex_plots = {
    "text.usetex": True,    
    "font.family": "serif",
    "axes.labelsize": 12,               
    "font.size": 12,
    "legend.fontsize": 10,               
    "xtick.labelsize": 12,
    "ytick.labelsize": 12,
}

mpl.rcParams.update(latex_plots)
```
