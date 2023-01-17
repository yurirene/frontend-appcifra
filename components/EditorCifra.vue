<template>
  <div>
    <button
      class="btn btn-xxs text-white bg-dark rounded-m "
      @click="editor.chain().focus().setHardBreak().run()">
      Quebrar Linha
    </button>
    <button
      class="btn btn-xxs text-white bg-dark rounded-m "
      @click="editor.chain().focus().toggleBold().run()"
    >
      Negrito
    </button>
    <editor-content :editor="editor" class="mt-3" />
  </div>
</template>

<script>
import { Editor, EditorContent } from '@tiptap/vue-2'
import StarterKit from '@tiptap/starter-kit'

export default {
  components: {
    EditorContent,
  },

  props: {
    value: {
      type: String,
      default: '',
    },
  },

  data() {
    return {
      editor: null,
    }
  },
  watch: {
    value(value) {
      const isSame = this.editor.getHTML() === value
      if (isSame) {
        return
      }

      this.editor.commands.setContent(value, false)
    },
  },

  mounted() {
    this.editor = new Editor({
      content: this.value ? this.value : 'Digite sua cifra',
      extensions: [
        StarterKit,
      ],
      onUpdate: () => {
        this.$emit('input', this.editor.getHTML())
      },
    })
  },

  beforeDestroy() {
    this.editor.destroy()
  },
}
</script>
