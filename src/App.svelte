<script>
  import Helper from "./Helper.svelte";

  let promise = getUnmigratedHelpers();

  function hasUnconvertedPlugin(mod) {
    return (
      Object.values(mod.entityList).some(
        (plugins) =>
          plugins.includes("ahorn") &&
          !plugins.includes("loenn") &&
          !plugins.includes("mlp")
      ) ||
      Object.values(mod.triggerList).some(
        (plugins) =>
          plugins.includes("ahorn") &&
          !plugins.includes("loenn") &&
          !plugins.includes("mlp")
      ) ||
      Object.values(mod.effectList).some(
        (plugins) =>
          plugins.includes("ahorn") &&
          !plugins.includes("loenn") &&
          !plugins.includes("mlp")
      )
    );
  }

  async function getUnmigratedHelpers() {
    const res = await fetch(
      `https://maddie480.ovh/celeste/custom-entity-catalog.json`
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
    This is a list of all Celeste helpers (all mods that aren't categorised as
    maps) that still have unmigrated plugins (that is, they have Ahorn plugins
    without corresponding Lönn plugins).
    <br />
    Note that some mods' Lönn support is provided through the
    <a href="https://gamebanana.com/tools/12000">More Lönn Plugins</a> plugin.
  </p>
  <p>
    This list is generated automatically (each time you load the page) from <a
      href="https://maddie480.ovh/celeste/custom-entity-catalog"
      >maddie480's Celeste Custom Entity and Trigger List</a
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
