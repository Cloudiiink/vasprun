# vasprun
This code is used for quick analysis of vasp calculation solely from vasprun.xml. It has the following features:

- band gap calculation
- dos plot (total dos / orbital partial dos / atomic partial dos)
- band structure plot (with color map enhancement)
- incar/potcar/poscar generation
- force analysys
- KS orbital eigenvalue analysis
- dynamical matrix (to add)
- elastic constants (to add)
- dielectric constants (to add)

## Prerequisites
To use it, one must have python 3 installed. It is recommended to install conda if one don't have root access in his computer.
In addition, several packages will be required
- `pymatgen`
- `pandas`
- `lxml`

One could follow the [wikipage](https://github.com/qzhu2017/CMS/wiki/Python-environment-setup) to set up your python environment.
```
$ python vasprun.py -h
```
## Usage
```
usage: vasprun.py [-h] [-i] [-p] [-c cif file] [-k] [-d dos_plot]
                  [-b band_plot] [-v vasprun] [-f] [-a] [-e] [-s smearing]
                  [-n figname] [-l lim] [-m max] [-o datname] [-q]

optional arguments:
  -h, --help            show this help message and exit
  -i, --incar           export incar file
  -p, --poscar          export poscar file
  -c cif file, --cif cif file
                        export symmetrized cif
  -k, --kpoints         export kpoint coordinate and its weight
  -d dos_plot, --dosplot dos_plot
                        export dos plot, options: t, spd, a, a-Si, a-1
  -b band_plot, --bandplot band_plot
                        export band plot, options: normal or projected
  -v vasprun, --vasprun vasprun
                        path of vasprun.xml file, default: vasprun.xml
  -f, --showforce       show forces
  -a, --allparameters   show all parameters
  -e, --eigenvalues     show eigenvalues in valence/conduction band
  -s smearing, --smear smearing
                        smearing parameter for dos plot, e.g., 0.1 A
  -n, --filename TEXT   dos/band figure/data file name, default:  fig.png

  -l, --lim <INT INT>   dos/band plot lim, default: -3 3
  -m, --plotmax FLOAT   band plot colorbar, default: 0.5
  -B, --bandexport      p4vasp-like export data of band structure
  -D, --dosexport       Export data of partial DOS
  -q, --quiet           In quiet mode, there will be no standard
                        output about the system
```
## Force information
```
$ python vasprun.py -v vasprun.xml -f yes
```
```
formula :   Sc4C4
efermi :   5.71888444
energy :   -65.82108919
metal :   True
gap :   -0.4621

+----+--------------+----------+-----------+
|    | functional   | labels   |   valence |
|----+--------------+----------+-----------|
|  0 | PBE          | Sc_sv    |        11 |
|  1 | PBE          | C        |         4 |
+----+--------------+----------+-----------+
+----+----------------------+-------------------------+
|    | lattice              | stress                  |
|----+----------------------+-------------------------|
|  0 | [3.478641, 0.0, 0.0] | [-4.62998221, 0.0, 0.0] |
|  1 | [0.0, 4.682565, 0.0] | [0.0, 1.73781963, 0.0]  |
|  2 | [0.0, 0.0, 6.176701] | [0.0, 0.0, 5.81088683]  |
+----+----------------------+-------------------------+
+----+--------------------------+---------------------------------+
|    | atom                     | force                           |
|----+--------------------------+---------------------------------|
|  0 | [0.0, 0.5, 0.606519]     | [0.0, 0.0, 0.02408274]          |
|  1 | [0.0, 0.0, 0.958081]     | [0.0, 0.0, 0.07723516]          |
|  2 | [0.5, 0.5, 0.041919]     | [0.0, 0.0, -0.07723516]         |
|  3 | [0.5, 0.0, 0.393481]     | [0.0, 0.0, -0.02408274]         |
|  4 | [0.5, 0.83775, 0.744469] | [0.0, 0.0691885, -0.00447732]   |
|  5 | [0.5, 0.16225, 0.744469] | [-0.0, -0.0691885, -0.00447732] |
|  6 | [0.0, 0.33775, 0.255531] | [0.0, 0.0691885, 0.00447732]    |
|  7 | [0.0, 0.66225, 0.255531] | [0.0, -0.0691885, 0.00447732]   |
+----+--------------------------+---------------------------------+

```
## DOS plots

A number of ways of plotting dos are also supported, some basic options are

- t: total dos
- spd: spd dos

one can just use the -d option to customize the plots.
If spin is included in vasprun.xml, the plot will show both up and down spin states separately.
```
$ python vasprun.py -v vasprun.xml -d t+spd -s 0.15 -n dos-spd.png
```
It will geneate a dos-spd.png figure as follows:
![dos.png](https://github.com/qzhu2017/vasprun/blob/master/images/dos.png)

## Band plots

```
$ python vasprun.py -v vasprun.xml-band -b normal -l -3 3 -m 0.4 -n band.png
```
It will generate a band.png figure as the follows
![band.png](https://github.com/qzhu2017/vasprun/blob/master/images/band.png)

Alternatively, one can generated the colored band based on the occupation of `projected DOS`.
```
$ python vasprun.py -v vasprun.xml-band -b -l -3 3 -m 0.4 -n band-projected.png
```
![band-projected.png](https://github.com/qzhu2017/vasprun/blob/master/images/band-projected.png)

## Band Export

Convert the `vasprun.xml` into readable k-E data. Default output is the k vector and coorespondent eigenvalue.

```
[0.0, 0.0, 0.0] -6.1975
```


