elf2fth
=======

A tool for generating Forth inline words from compiled object code (ELF files).
Generated words are to be used with [Mecrisp Forth](http://mecrisp.sourceforge.net/).

Example
-------

Input assembly source file - `test.S`:

<pre><code>
  .syntax unified
  .cpu    cortex-m0
  .thumb
  .text

  nop          @ placeholder for "push {lr}"

  ldr  r0, =0x12345678
loop:
  subs r0, #1
  bne  loop

  .end
</code></pre>

Compilation :

`$ arm-none-eabi-as -o test.o test.S`

Forth code generation :

`$ elf2fth -s -w test-1 test.o > test.forth`

Generated word - `test.forth` :

<pre><code>
: test-1 [
  $4801 h, $3801 h, $D1FD h, $5678 h,
  $1234 h, ] ;
</code></pre>
