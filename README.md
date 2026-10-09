# Supernova Chemical Yield Tables

This repository provides elemental yield tables for supernova progenitor models with different initial masses, metallicities, and rotation velocities. The tables contain initial abundances, production factors, total elemental yields, and net yields for use in chemical enrichment studies.

**If you use these data in your work, please cite [Marassi et al. (2019)](https://doi.org/10.1093/mnras/sty3323).** The full reference and a BibTeX entry are provided below.

## 1. Model Grid and File Names

All 102 data files are stored in the repository root, with names of the form:

```text
<mass><metallicity><velocity>.yele_dec
```

For example, `013a000.yele_dec` identifies a 13 M_sun progenitor with [Fe/H] = 0 and an initial rotation velocity of 0 km/s.

| File-name component | Values | Meaning |
| --- | --- | --- |
| `<mass>` | `013`, `015`, `020`, `025`, `030`, `040`, `060`, `080`, `120` | Initial progenitor mass in solar masses (M_sun) |
| `<metallicity>` | `a`, `b`, `c`, `d` | Metallicity code; see the mapping below |
| `<velocity>` | `000`, `150`, `300` | Initial rotation velocity in km/s |

### Metallicity Mapping and Available Masses

| Code | [Fe/H] | Available progenitor masses [M_sun] |
| --- | --- | --- |
| `a` | 0 | 13, 15, 20, 25, 30, 40, 60, 80, 120 |
| `b` | -1 | 13, 15, 20, 25, 30, 40, 60, 80, 120 |
| `c` | -2 | 13, 15, 20, 25, 30, 40, 60, 80 |
| `d` | -3 | 13, 15, 20, 25, 30, 40, 60, 80 |

Each available mass and metallicity combination includes all three rotation velocities. The 120 M_sun models at [Fe/H] = -2 and -3 are not included in this repository.

## 2. Data Format

Each `.yele_dec` file is a whitespace-separated text table with one header line and 53 elemental entries. The header is:

```text
nome    Z   initial       PF         YieldTot Newly Produced
```

| Column | Header | Description | Unit |
| --- | --- | --- | --- |
| 1 | `nome` | Chemical element symbol | — |
| 2 | `Z` | Atomic number | — |
| 3 | `initial` | Initial mass fraction of the element | Dimensionless |
| 4 | `PF` | Production factor: ejected mass fraction divided by initial mass fraction | Dimensionless |
| 5 | `YieldTot` | Total ejected mass of the element | M_sun |
| 6 | `Newly Produced` | Net yield, after subtracting the initial elemental content of the ejected material | M_sun |

`Z` in the table denotes atomic number, not stellar metallicity. `Newly Produced` is a single column label containing a space, so there are six data columns despite the seven whitespace-separated words in the header.

Negative net yields indicate net destruction of an element. Use `YieldTot` when the total ejected elemental mass is needed, and `Newly Produced` when the net production is needed.

The tables include H through Mo (atomic numbers 1–42), Xe through Nd (54–60), and Hg through Bi (80–83). Elements outside these ranges are not tabulated.

## 3. Reading the Data

The following example uses the Python standard library and can be run from the repository root:

```python
from pathlib import Path

path = Path("013a000.yele_dec")
yields = {}

with path.open() as table:
    next(table)  # Skip the header; "Newly Produced" is one column.
    for line in table:
        element, atomic_number, initial, pf, total, net = line.split()
        yields[element] = {
            "atomic_number": int(atomic_number),
            "initial_mass_fraction": float(initial),
            "production_factor": float(pf),
            "total_yield_msun": float(total),
            "net_yield_msun": float(net),
        }

print("Total oxygen yield [M_sun]:", yields["O"]["total_yield_msun"])
```

## 4. Model Description and Citation

For the scientific context and the effects of metallicity, rotation, and fallback on supernova yields, refer to:

S. Marassi, R. Schneider, M. Limongi, A. Chieffi, L. Graziani, and S. Bianchi (2019), **“Supernova dust yields: the role of metallicity, rotation, and fallback,”** *Monthly Notices of the Royal Astronomical Society*, **484**(2), 2587–2604. [DOI: 10.1093/mnras/sty3323](https://doi.org/10.1093/mnras/sty3323).

Please cite this paper in any publication or other work that uses the data in this repository.

```bibtex
@article{Marassi2019,
  author  = {Marassi, S. and Schneider, R. and Limongi, M. and
             Chieffi, A. and Graziani, L. and Bianchi, S.},
  title   = {Supernova dust yields: the role of metallicity, rotation, and fallback},
  journal = {Monthly Notices of the Royal Astronomical Society},
  year    = {2019},
  volume  = {484},
  number  = {2},
  pages   = {2587--2604},
  doi     = {10.1093/mnras/sty3323},
  url     = {https://doi.org/10.1093/mnras/sty3323}
}
```

## 5. Contact

For questions about the files or to report a problem, please open an issue in this repository.
