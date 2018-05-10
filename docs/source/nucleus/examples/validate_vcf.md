# nucleus.examples.validate_vcf -- Validates that a VCF file and a FASTA reference file correspond.
**Source code:** [nucleus/examples/validate_vcf.py](https://github.com/google/nucleus/tree/master/nucleus/examples/validate_vcf.py)
---
They correspond if:
a) they cover the same contigs,
b) the reference covers every variant in the vcf file, and
c) they agree on the reference bases covered by the variants.

## Functions overview
Name | Description
-----|------------
[`main`](#main)`(argv)` | 
[`validate_contigs`](#validate_contigs)`(ref_contigs, vcf_contigs)` | Validate that the two lists of ContigInfos have the same set of names.
[`validate_variant`](#validate_variant)`(ref_reader, variant)` | Validate that variant is covered by the reference and agrees with it.

## Functions
###<a name="<_ast.FunctionDef object at 0x555808fcaa10>"></a> main(argv)


###<a name="<_ast.FunctionDef object at 0x555808fce5d0>"></a> validate_contigs(ref_contigs, vcf_contigs)
Validate that the two lists of ContigInfos have the same set of names.

###<a name="<_ast.FunctionDef object at 0x555808fcda90>"></a> validate_variant(ref_reader, variant)
Validate that variant is covered by the reference and agrees with it.

