<script>
  import Clipboard from "svelte-clipboard";
  import { vscode } from "../services/index";
  export let labelType = "iconClassname";
  export let iconCode;
  export let iconUnicode;
  export let iconLabel;
  export let iconStyle;
  export let iconStylePrefix;
</script>

<Clipboard
  text={labelType === "iconClassname" ? '<i class="'+iconCode+'"></i>' : 'content: "\\'+iconUnicode+'";'}
  let:copy
  on:copy={() => {
    vscode.postMessage({
      command: "onInfo",
      content: {
        message: "The icon code has been copied...",
      },
    });
  }}
>
  <div
    role="button"
    class="listItem"
    on:click={copy}
  >
    <span class="inner">
      <div class="col col-2 align-middle">
        <div class="icon-container col col-2 ml1 align-middle"><i class={iconCode} /></div>
      </div>
      <div class="name-container col col-10">
        <div class="name-container">
          <span class="icon-label">{iconLabel}</span>
          <span class="icon-style">{iconStyle}</span>
        </div>
        <div>
          {#if labelType === "iconClassname"}
            <code>{iconCode}</code>
          {:else if labelType === "iconUnicode"}
            <code>{iconUnicode}</code>
          {/if}
        </div> 
      </div>
    </span>
  </div>
</Clipboard>

<style>
  .listItem {
    width: 100%;
    margin-bottom: 5px;
  }

  .listItem .inner:hover {
    background-color: var(--vscode-textSeparator-foreground);
  }

  .listItem .inner {
    width: 100%;
    height: fit-content;
    align-items: center;
    padding-top: 5px;
    padding-bottom: 5px;
    background-color: var(--vscode-input-background);
    box-shadow: 0px 0px 12px rgb(0 0 0 / 6%);
    transition: all 0.3s ease-in-out;
    display: flex;
  }

  .listItem .inner .name-container {
    margin: 0px 5px 0px 5px;
    overflow: scroll;
    margin-top: auto;
    margin-bottom: auto;
  }

  .listItem .inner .name-container::-webkit-scrollbar {
    display: none;
  }

  .listItem .inner i {
    font-size: 10vw;
    /* padding: 10px 10px 2px 10px; */
  }

  .listItem .inner .icon-label {
    font-size: 3vw !important;
    font-family: var(--vscode-editor-font-family);
    color: var(--vscode-foreground);
  }

  .listItem .inner .icon-style {
    font-size: 2vw !important;
    font-family: var(--vscode-editor-font-family);
    color: var(--vscode-foreground);
    text-transform: uppercase;
  }
  .listItem .inner code {
    font-size: 3vw;
    max-width: 100%;
    padding: 2px;
    border-radius: 3px;
    background-color: var(--vscode-editor-background);
  }

  .listItem .icon-container {
    margin-top: auto;
    margin-bottom: auto;
    text-align: center;
  }
</style>
