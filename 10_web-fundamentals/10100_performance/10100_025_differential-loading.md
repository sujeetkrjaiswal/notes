# Differential Loading

description: Differencial Loading

```markup
<script type="module">
  console.log("Runs in modern browsers");
</script>

<script nomodule>
  console.log("Modern browsers know both type=module and nomodule, so skip this")
  console.log("Old browsers ignore script with unknown type=module, but execute this.");
</script>
```

