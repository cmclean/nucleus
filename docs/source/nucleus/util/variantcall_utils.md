# nucleus.util.variantcall_utils -- VariantCall utilities.
**Source code:** [nucleus/util/variantcall_utils.py](https://github.com/google/nucleus/tree/master/nucleus/util/variantcall_utils.py)
---


## Functions overview
Name | Description
-----|------------
[`get_ad`](#get_ad)`(variant_call)` | Gets the allele depth of the VariantCall.
[`get_format`](#get_format)`(variant_call, field_name, vcf_object=None)` | Returns the value of the `field_name` FORMAT field.
[`get_gl`](#get_gl)`(variant_call)` | Returns the genotype likelihoods of the VariantCall.
[`get_gq`](#get_gq)`(variant_call)` | Gets the genotype quality of the VariantCall.
[`get_gt`](#get_gt)`(variant_call)` | Returns the genotypes of the VariantCall.
[`get_min_dp`](#get_min_dp)`(variant_call)` | Gets the 'MIN_DP' field of the VariantCall.
[`has_genotypes`](#has_genotypes)`(variant_call)` | Returns True iff the VariantCall has one or more called genotypes.
[`ploidy`](#ploidy)`(variant_call)` | Returns the ploidy of the VariantCall.
[`set_ad`](#set_ad)`(variant_call, ad)` | Sets the allele depth of the VariantCall.
[`set_format`](#set_format)`(variant_call, field_name, value, vcf_object=None)` | Sets a field of the info map of the `VariantCall` to the given value(s).
[`set_gl`](#set_gl)`(variant_call, gl)` | Sets the genotype likelihoods of the VariantCall.
[`set_gq`](#set_gq)`(variant_call, gq)` | Sets the genotype quality of the VariantCall.
[`set_gt`](#set_gt)`(variant_call, gt)` | Sets the genotypes of the VariantCall.
[`set_min_dp`](#set_min_dp)`(variant_call, min_dp)` | Sets the 'MIN_DP' field of the VariantCall.

## Functions
### `get_ad(variant_call)`<a name="get_ad"></a>
```
Gets the allele depth of the VariantCall.
```

### `get_format(variant_call, field_name, vcf_object=None)`<a name="get_format"></a>
```
Returns the value of the `field_name` FORMAT field.

The `vcf_object` is used to determine the type of the resulting value. If it
is a single value or a Flag, that single value will be returned. Otherwise,
the list of values is returned.

Args:
  variant_call: VariantCall proto. The VariantCall of interest.
  field_name: str. The name of the field to retrieve values from.
  vcf_object: (Optional) A VcfReader or VcfWriter object. If not None, the
    type of the field is inferred from the associated VcfReader or VcfWriter
    based on its name. Otherwise, the type is inferred if it is a reserved
    field.
```

### `get_gl(variant_call)`<a name="get_gl"></a>
```
Returns the genotype likelihoods of the VariantCall.

Args:
  variant_call: VariantCall proto. The VariantCall for which to return GLs.

Returns:
  A list of floats representing the genotype likelihoods of this call.
```

### `get_gq(variant_call)`<a name="get_gq"></a>
```
Gets the genotype quality of the VariantCall.
```

### `get_gt(variant_call)`<a name="get_gt"></a>
```
Returns the genotypes of the VariantCall.

Args:
  variant_call: VariantCall proto. The VariantCall for which to return GTs.

Returns:
  A list of ints representing the genotype indices of this call.
```

### `get_min_dp(variant_call)`<a name="get_min_dp"></a>
```
Gets the 'MIN_DP' field of the VariantCall.
```

### `has_genotypes(variant_call)`<a name="has_genotypes"></a>
```
Returns True iff the VariantCall has one or more called genotypes.

Args:
  variant_call: VariantCall proto. The VariantCall to evaluate.

Returns:
  True if the VariantCall has one or more called genotypes, False otherwise.
```

### `ploidy(variant_call)`<a name="ploidy"></a>
```
Returns the ploidy of the VariantCall.

Args:
  variant_call: VariantCall proto. The VariantCall to evaluate.

Returns:
  The ploidy of the call (a non-negative integer).
```

### `set_ad(variant_call, ad)`<a name="set_ad"></a>
```
Sets the allele depth of the VariantCall.
```

### `set_format(variant_call, field_name, value, vcf_object=None)`<a name="set_format"></a>
```
Sets a field of the info map of the `VariantCall` to the given value(s).

`variant_call.info` is analogous to the FORMAT field of a VCF call.

Example usage:
with vcf.VcfReader('/path/to/my.vcf') as vcf_reader:
  for variant in vcf_reader:
    first_call = variant.calls[0]
    # Type can be inferred for reserved VCF fields.
    set_format(first_call, 'AD', 25)
    # Specify the reader explicitly for unknown fields.
    set_format(first_call, 'MYFIELD', 30, vcf_reader)

Args:
  variant_call: VariantCall proto. The VariantCall to modify.
  field_name: str. The name of the field to set.
  value: A single value or list of values to update the VariantCall with.
    The type of the value is determined by the `vcf_object` if one is given,
    otherwise is looked up based on the reserved FORMAT fields in the VCF
    specification.
  vcf_object: (Optional) A VcfReader or VcfWriter object. If not None, the
    type of the field is inferred from the associated VcfReader or VcfWriter
    based on its name. Otherwise, the type is inferred if it is a reserved
    field.
```

### `set_gl(variant_call, gl)`<a name="set_gl"></a>
```
Sets the genotype likelihoods of the VariantCall.

Args:
  variant_call: VariantCall proto. The VariantCall to modify.
  gl: list(float). The list of genotype likelihoods for the VariantCall.
```

### `set_gq(variant_call, gq)`<a name="set_gq"></a>
```
Sets the genotype quality of the VariantCall.
```

### `set_gt(variant_call, gt)`<a name="set_gt"></a>
```
Sets the genotypes of the VariantCall.

Args:
  variant_call: VariantCall proto. The VariantCall to modify.
  gt: list(int). The list of genotypes for the VariantCall.
```

### `set_min_dp(variant_call, min_dp)`<a name="set_min_dp"></a>
```
Sets the 'MIN_DP' field of the VariantCall.
```

