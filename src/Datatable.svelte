<script>
  import { areas } from "./stores";
</script>

{#if $areas}
  <table class="table is-fullwidth">
    <thead>
      <tr>
        <th>Programme</th>
        <th>Code</th>
        <th>Nom</th>
        <th>Superficie</th>
      </tr>
    </thead>
    <tbody>
      {#each $areas as area}
        <tr>
          <td>{area.program}</td>
          <td>{area.code}</td>
          <td>{area.name}</td>
          <td>{area.surface}</td>
        </tr>
      {/each}
    </tbody>
  </table>
{/if}
