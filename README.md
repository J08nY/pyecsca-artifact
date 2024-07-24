# pyecsca: Reverse engineering black-box elliptic curve cryptography via side-channel analysis

This artifact accompanies the *"pyecsca: Reverse engineering black-box elliptic curve cryptography via side-channel analysis"*
paper and contains:
 - The pyecsca toolkit itself, as a git submodule, implementing the methods presented in the paper. This is under [pyecsca/](pyecsca/) and also
   in the repository [https://github.com/J08nY/pyecsca](https://github.com/J08nY/pyecsca).
 - The notebooks used to evaluate the methods in the paper. This is under [pyecsca/notebook/](pyecsca/notebook/) and also
   in the repository [https://github.com/J08nY/pyecsca-notebook](https://github.com/J08nY/pyecsca-notebook).
 - The documentation for the toolkit. This is under [docs/](docs/) and also on the site [https://pyecsca.org](https://pyecsca.org).
 - The expanded EFD formula dataset as described in the paper. This is under [expanded_efd/](expanded_efd/) and also
   on [https://zenodo.org/records/10908698](https://zenodo.org/records/10908698).

To reproduce the results in the paper follow these instructions. All of the tables, figures and datasets
are reproduced here. If you with to explore the toolkit more, use the notebooks linked next to sections in the paper
or at [https://pyecsca.org/notebooks.html](https://pyecsca.org/notebooks.html).

We performed the evaluation on a machine with Ubuntu 23.04, 64GB RAM, and AMD Ryzen 9 7950X 16-Core CPU. However,
a machine with much less resources should be perfectly usable, yet slower.

### Installation

Follow the [installation docs](https://pyecsca.org/installation.html) and install the core package as well
as the requirements for the notebooks (they can be found in `requirements.txt` in the notebook repository).

```shell
python3 -m venv virt
. virt/bin/activate
pip install pyecsca/
pip install -r pyecsca/notebook/requirements.txt
```

Then run either Jupyter Lab or Notebook:
```shell
jupyter lab
```

### Formula expansion (Section 3.5)

Assuming the **pyecsca** package is installed, open the `formulas.ipynb` Jupyter notebook in the
`pyecsca/notebook/re/` directory. Run its cells sequentially and observe the expanded formula
dataset generated in the final cell. Change the `num_workers` value to something that
suits your setup (i.e. number of your CPU cores minus two).

### Configuration space (Table 2)

Assuming the **pyecsca** package is installed, open the `configuration_space.ipynb` Jupyter notebook in the
`pyecsca/notebook/` directory. Run its cells sequentially and observe the tables being printed
(with additional columns that are not present in the paper). Note that the cell computing all of the configurations
takes some time to finish (~ 10 minutes), as it enumerates them internally.

### RPA evaluation (Section 6.1 and Table 3)

Assuming the **pyecsca** package is installed, open the `rpa.ipynb` Jupyter notebook in the
`pyecsca/notebook/re/` directory. Run its cells sequentially. Change the `num_workers` value to something that
suits your setup (i.e. number of your CPU cores minus two).

You can observe the tree metrics for RPA-RE in Table 3 from the following cell:

```python
re = RPA(set(multipliers))
with silent():
    re.build_tree(p256, tries=10)
print(re.tree.describe())
```

You can observe Figures 5 and 6 as direct output of some cells, as well as saved into files:
 - *rpa_re_success_rate_symmetric.pdf* and *rpa_re_query_rate_symmetric.pdf* for Figure 5
 - *rpa_re_errors.pdf* for Figure 6.

### ZVP evaluation (Section 6.2 and Table 3)

Assuming the **pyecsca** package is installed, open the `zvp.ipynb` Jupyter notebook in the
`pyecsca/notebook/re/` directory. Run its cells sequentially, upto and including the
evaluation part. Optionally, you can skip some
cells that are there to store and subsequently load various intermediate datasets. These can, 
however, help you save your progress if something happens to the computation later on.
Change the `num_workers` value to something that suits your setup (i.e. number of your CPU cores minus two).

You can observe the tree metrics for ZVP-RE in Table 3 from the following cell:

```python
print("Zero hit")
print(tree_remapped.describe())
print("\nZero count")
print(tree_count.describe())
print("\nZero position")
print(tree_position.describe())
```

You can observe Figures 7 and 8 as direct output of some cells, as well as saved into files:
 - *zvp_re_success_rate_symmetric.pdf*, *zvp_re_query_rate_symmetric.pdf*, *zvp_re_precise_rate_symmetric.pdf*, and *zvp_re_amount_rate_symmetric.pdf* for Figure 7
 - *zvp_re_success_rate_binomial.pdf*, *zvp_re_query_rate_binomial.pdf*, *zvp_re_precise_rate_binomial.pdf*, and *zvp_re_amount_rate_binomial.pdf* for Figure 8
