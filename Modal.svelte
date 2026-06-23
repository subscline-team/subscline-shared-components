<script lang="ts">
  import { fade } from 'svelte/transition'
  import closeIcon from './close.svg'
  import CommonCss from './CommonCss.svelte'
  import Divider from './Divider.svelte'
  import IconButton from './IconButton.svelte'
  import Portal from './Portal.svelte'
  import { lockBodyScroll } from './utils'

  export let opened = false
  export let persistent = false
  export let showCloseButton = false
  export let centeringTitle = false
  export let onClose: (() => unknown) | undefined = undefined
  export let style: string | undefined = undefined
  let klass = ''
  export { klass as class }

  const open = () => (opened = true)
  const close = () => {
    opened = false
    onClose?.()
  }
  const toggle = () => {
    opened ? close() : open()
  }

  // バックドロップ上で mousedown が始まったかを記録するフラグ。
  // click イベントは mousedown と mouseup の「共通祖先」で発火するため、
  // フレーム内で押下 → バックドロップ上で離す（あるいはその逆のドラッグ）でも
  // click はバックドロップ (.root) で発火してしまい、誤って閉じてしまう。
  // それを防ぐため、押下と離上の両方がバックドロップ自身だったときだけ閉じる。
  let backdropMouseDown = false

  function onBackdropMouseDown(event: MouseEvent) {
    // mousedown がバックドロップ (.root 自身) 上で始まったかだけを記録する。
    backdropMouseDown = event.target === event.currentTarget
  }

  function onBackdropMouseUp(event: MouseEvent) {
    // mousedown も mouseup もバックドロップ自身のときだけ閉じる。
    // (内側 mousedown → 外側 mouseup / 外側 mousedown → 内側 mouseup はいずれも閉じない)
    const shouldClose = backdropMouseDown && event.target === event.currentTarget
    backdropMouseDown = false
    if (shouldClose && !persistent) {
      close()
    }
  }
</script>

<slot name="launcher" {open} {close} {toggle} />
{#if opened}
  <Portal>
    <div
      class="root"
      transition:fade={{ duration: 150 }}
      on:mousedown={onBackdropMouseDown}
      on:mouseup={onBackdropMouseUp}
      {...$$restProps}
    >
      <slot name="frame" {open} {close} {toggle}>
        <div class={`frame ${klass}`} {style}>
          <div class="flex items-center justify-between py-1.5" class:hidden={!$$slots.title && !showCloseButton}>
            {#if centeringTitle}
              <!-- タイトルのきちんと中央寄せしつつ、長いテキストを適切に折り返すために左右に同サイズの要素を置く -->
              <IconButton class="invisible" src={closeIcon} />
            {/if}
            <span class="px-4 text-base font-bold" class:text-center={centeringTitle}>
              <slot name="title" />
            </span>
            <IconButton class="mx-1.5" classes={{ invisible: !showCloseButton }} src={closeIcon} onClick={close} />
          </div>
          <Divider classes={{ hidden: !$$slots.title }} />

          <div class="overflow-auto" use:lockBodyScroll>
            <slot {open} {close} {toggle} />
          </div>

          <div class="footer">
            <slot name="footer" />
          </div>
        </div>
      </slot>
    </div>
  </Portal>
{/if}

<CommonCss />

<style lang="postcss">
  .root {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    z-index: var(--modal-backdrop-z-index);

    display: flex;
    align-items: center;
    justify-content: center;

    background-color: hsla(0, 0%, 0%, 55%);
  }

  .frame {
    @apply grid bg-white;
    grid-template-rows: auto auto minmax(0, 1fr) auto auto;
    max-height: 90vh;
    max-width: 90vw;

    border-radius: 0.5rem;
    box-shadow: 0 3px 14px hsla(0, 0%, 0%, 20%);
  }

  .footer {
    @apply p-2 empty:hidden;
    box-shadow: 0px -2px 4px rgba(0, 0, 0, 0.05);
  }
</style>
