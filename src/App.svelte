<script>
  import Helper from "./Helper.svelte";

  let promise = getUnmigratedHelpers();

  function hasUnconvertedPlugin(mod) {
    return (
      Object.values(mod.entityList).some(
        (plugins) => plugins.includes("ahorn") && !plugins.includes("loenn")
      ) ||
      Object.values(mod.triggerList).some(
        (plugins) => plugins.includes("ahorn") && !plugins.includes("loenn")
      ) ||
      Object.values(mod.effectList).some(
        (plugins) => plugins.includes("ahorn") && !plugins.includes("loenn")
      )
    );
  }

  async function getUnmigratedHelpers() {
    const res = await fetch(
      `https://max480.ovh/celeste/custom-entity-catalog.json`
    );

    if (res.ok) {
      let catalog = await res.json();
      let mods = catalog.modInfo;
      let helpers = mods.filter(
        (mod) => mod.categoryName != "Map" && hasUnconvertedPlugin(mod)
      );
      return helpers;
    } else {
      throw new Error(await res.text());
    }
  }
</script>

<main>
  <h1>Celeste Lönn Migration Index</h1>

  <p>
    This is a list of all Celeste helpers (all mods that aren't categorised as maps) that still have unmigrated plugins
    (that is, they have Ahorn plugins without corresponding Lönn plugins).
  </p>
  <p>
    This list is generated automatically (each time you load the page) from <a
      href="https://max480.ovh/celeste/custom-entity-catalog"
      >max480's Celeste Custom Entity and Trigger List</a
    >, so it updates when that updates.
  </p>

  {#await promise}
    <p class="loading">Loading...</p>
  {:then helpers}
    {#each helpers as mod (mod.itemid)}
      <Helper {mod} />
    {/each}
  {:catch error}
    <h3>
      An error occurred when trying to load the Celeste Custom Entity and
      Trigger List:
    </h3>
    <pre><code style="color: red">{error.message}</code></pre>
  {/await}
</main>

<style>
  .loading {
    color: var(--text-muted);
  }
</style>
