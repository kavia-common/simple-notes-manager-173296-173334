<script lang="ts">
  import Header from '$lib/components/Header.svelte';
  import NoteList from '$lib/components/NoteList.svelte';
  import NoteEditor from '$lib/components/NoteEditor.svelte';
  import EmptyState from '$lib/components/EmptyState.svelte';
  import { notes, createNote, updateNote, deleteNote, getNoteById } from '$lib/storage';

  let selectedId = $state<string | null>(null);

  function handleNewNote() {
    const n = createNote('Untitled', '');
    selectedId = n.id;
  }

  function handleSelect(id: string) {
    selectedId = id;
  }

  function handleDelete(id: string) {
    const shouldDelete = confirm('Delete this note? This cannot be undone.');
    if (!shouldDelete) return;
    deleteNote(id);
    if (selectedId === id) {
      // choose next available note
      const arr = $notes;
      selectedId = arr.length > 0 ? arr[0].id : null;
    }
  }

  function handleSave(title: string, content: string) {
    if (!selectedId) return;
    updateNote(selectedId, { title, content });
  }

  $effect.root(() => {
    // Initialize default selection if notes exist
    const unsub = notes.subscribe((arr) => {
      if (!selectedId && arr.length) {
        selectedId = arr[0].id;
      } else if (selectedId) {
        // ensure selected still exists
        const exists = arr.some((n) => n.id === selectedId);
        if (!exists) selectedId = arr.length ? arr[0].id : null;
      }
    });
    return () => unsub();
  });
</script>

<svelte:head>
  <title>Simple Notes Manager</title>
  <meta name="description" content="Create, edit, and manage personal notes. Ocean Professional theme." />
</svelte:head>

<Header onNewNote={handleNewNote} />

<div class="layout">
  <div class="sidebar-wrap">
    <NoteList
      notes={$notes}
      selectedId={selectedId}
      onSelect={handleSelect}
      onDelete={handleDelete}
    />
  </div>

  <div class="main-panel">
    {#if $notes.length === 0}
      <EmptyState onNewNote={handleNewNote} />
    {:else}
      <NoteEditor
        note={getNoteById($notes, selectedId)}
        onSave={handleSave}
      />
    {/if}
  </div>
</div>

<style>
  .layout {
    max-width: 1200px;
    width: 100%;
    margin: 18px auto 28px;
    padding: 0 16px;
    display: grid;
    grid-template-columns: 360px 1fr;
    gap: 16px;
  }

  .sidebar-wrap {
    position: sticky;
    top: 72px;
    align-self: start;
    height: calc(100vh - 120px);
    overflow: hidden;
  }

  .sidebar-wrap :global(.sidebar) {
    height: 100%;
    overflow: hidden;
    display: grid;
    grid-template-rows: auto 1fr;
  }

  .sidebar-wrap :global(.list) {
    overflow: auto;
  }

  .main-panel {
    border-radius: var(--radius);
    border: 1px solid var(--border);
    background: var(--surface);
    box-shadow: var(--shadow);
    min-height: 360px;
  }

  @media (max-width: 900px) {
    .layout {
      grid-template-columns: 1fr;
    }
    .sidebar-wrap {
      position: static;
      height: auto;
    }
  }
</style>
