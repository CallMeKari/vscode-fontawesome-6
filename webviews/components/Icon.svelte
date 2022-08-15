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
    class="icon align-middle"
    on:click={copy}
  >
    <span class="inner">
      <i class={iconCode}></i>
      <div class="text-center name-container icon-label">{iconLabel}</div>
      <div class="text-center name-container icon-style">{iconStyle}</div>
      <div class="text-center name-container">
        {#if labelType === "iconClassname"}
          <code>{iconCode}</code>
        {:else if labelType === "iconUnicode"}
          <code>{`\\${iconUnicode}`}</code>
        {/if}
      </div>
    </span>
  </div>
</Clipboard>

<style>
  .icon {
    display: inline-block;
    margin-bottom: 5px;
    width: 32%;
  }

  @media only screen and (max-width: 250px) {
    .icon {
      width: 49% !important;
    }
  }

  @media only screen and (max-width: 215px) {
    .icon {
      width: 99% !important;
    }

    .icon .inner {
      height: 100% !important;
    }

    .icon .inner i {
      font-size: 10vw !important;
      padding: 10px 10px 3% 10px !important;
    }

    .icon .inner code {
      font-size: 2vw !important;
    }
  }

  .icon .inner:hover {
    background-color: var(--vscode-textSeparator-foreground);
  }

  .icon .inner {
    display: inline-block;
    text-align: center;
    width: 100%;
    background-color: var(--vscode-input-background);
    box-shadow: 0px 0px 12px rgb(0 0 0 / 6%);
    transition: all 0.3s ease-in-out;
    padding-bottom: 10px;
  }

  .icon .inner .name-container {
    margin: 0px 5px 0px 5px;
    overflow: scroll;
  }

  .icon .inner .icon-label {
    font-size: 3vw !important;
    font-family: var(--vscode-editor-font-family);
    color: var(--vscode-foreground);
  }

  .icon .inner .icon-style {
    font-size: 2vw !important;
    font-family: var(--vscode-editor-font-family);
    color: var(--vscode-foreground);
    text-transform: uppercase;
  }

  .icon .inner .name-container::-webkit-scrollbar {
    display: none;
  }

  .icon .inner i {
    font-size: 10vw;
    padding: 10px 10px 3% 10px;
  }

  .icon .inner code {
    font-size: 2vw;
    max-width: 100%;
    padding: 2px;
    border-radius: 3px;
    background-color: var(--vscode-editor-background);
  }
</style>
