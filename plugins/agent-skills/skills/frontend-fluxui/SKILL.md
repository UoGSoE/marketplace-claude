---
name: frontend-design-with-flux
description: Create production-grade frontend interfaces with high design quality. Use this skill when the user asks to build web components, pages, or applications which are using livewire / flux.
---

# Flux UI v2 Quick Reference

> Flux is a component library for Livewire built with Tailwind CSS v4.

## Core Principles

### We Style, You Space
Flux provides **styling** (padding, colors). You provide **spacing** (margins, gaps, layout):

```blade
<form class="space-y-6">
    <flux:input label="Email" wire:model="email" />
    <flux:input label="Password" wire:model="password" />
    <flux:button type="submit" variant="primary">Submit</flux:button>
</form>
```

### Composition Over Configuration
Components mix and match. A `<flux:button>` works standalone OR inside other components.

---

## Common Mistakes to Avoid

### Use Flux Typography Components
```blade
{{-- WRONG: raw HTML with custom Tailwind --}}
<p class="text-sm text-zinc-500">Some help text</p>
<a href="/profile" class="text-blue-600 hover:underline">Profile</a>

{{-- CORRECT: Flux components --}}
<flux:text>Some help text</flux:text>
<flux:link href="/profile">Profile</flux:link>
```

### Button Variants - Less is More
Most buttons should be unstyled. Only the primary action gets `variant="primary"`:

```blade
{{-- WRONG: rainbow of variants --}}
<div class="flex gap-2">
    <flux:button variant="subtle">Cancel</flux:button>
    <flux:button variant="outline">Preview</flux:button>
    <flux:button variant="filled">Draft</flux:button>
    <flux:button variant="primary">Save</flux:button>
</div>

{{-- CORRECT: simple and clear --}}
<div class="flex gap-2">
    <flux:button>Cancel</flux:button>
    <flux:button>Preview</flux:button>
    <flux:button variant="primary">Save</flux:button>
</div>
```

Only use `variant="danger"` for genuinely destructive actions (delete, remove).

### Modals - Default to Flyout
```blade
{{-- Prefer flyout variant --}}
<flux:modal name="filters" variant="flyout">
    ...
</flux:modal>
```

### Modals - New trigger bahaviour
In flux v2, modals can be triggered by a `<flux:modal.trigger>` component with the name prop matchers rather than toggling component state or events.

```blade
<flux:modal.trigger name="edit-profile">
    <flux:button>Edit</flux:button>
</flux:modal.trigger>

<flux:modal name="edit-profile" class="md:w-96">
    <div class="space-y-6">
       ....
    </div>
</flux:modal>
```

### Colours - Use Sparingly
Badges, callouts etc have colour options - but only use them when genuinely needed to draw attention. Default/neutral is usually fine.

---

## Component Naming Patterns

### Groups (.group suffix) - standalone components that CAN be grouped
```blade
<flux:button.group>
    <flux:button>One</flux:button>
    <flux:button>Two</flux:button>
</flux:button.group>

<flux:checkbox.group>...</flux:checkbox.group>
<flux:radio.group>...</flux:radio.group>
```

### Items (.item suffix) - components that MUST be inside a parent
```blade
<flux:menu>
    <flux:menu.item>Edit</flux:menu.item>
</flux:menu>

<flux:navlist>
    <flux:navlist.item href="#" icon="home">Home</flux:navlist.item>
</flux:navlist>
```

---

## Table Component (v2 Syntax)

This changed significantly from v1. Use this structure:

```blade
<flux:table>
    <flux:table.columns>
        <flux:table.column>Name</flux:table.column>
        <flux:table.column>Email</flux:table.column>
        <flux:table.column>Status</flux:table.column>
    </flux:table.columns>

    <flux:table.rows>
        @foreach ($users as $user)
            <flux:table.row wire:key="user-row-{{ $user->id }}">
                <flux:table.cell>{{ $user->name }}</flux:table.cell>
                <flux:table.cell>{{ $user->email }}</flux:table.cell>
                <flux:table.cell>
                    <flux:badge size="sm" inset="top bottom">Active</flux:badge>
                </flux:table.cell>
            </flux:table.row>
        @endforeach
    </flux:table.rows>
</flux:table>
```

Note: `flux:table.columns` and `flux:table.rows` are required wrappers. Always use `wire:key` on rows.

---

## Form Shorthand

Always prefer flux:input shorthand syntax over the verbose flux:field syntax.  Inputs accept `label` and `description` props directly:

```blade
{{-- Shorthand --}}
<flux:input label="Email" description="We won't share this" wire:model="email" />

{{-- Expands to --}}
<flux:field>
    <flux:label>Email</flux:label>
    <flux:description>We won't share this</flux:description>
    <flux:input wire:model="email" />
    <flux:error name="email" />
</flux:field>
```

---

## Select Component

```blade
{{-- Basic native select --}}
<flux:select wire:model="country" label="Country">
    <flux:select.option value="">Choose...</flux:select.option>
    <flux:select.option value="uk">United Kingdom</flux:select.option>
</flux:select>

{{-- Searchable/multiple (Pro) --}}
<flux:select variant="listbox" searchable multiple>
    <flux:select.option>Option 1</flux:select.option>
    <flux:select.option>Option 2</flux:select.option>
    <flux:select.option>Option 3</flux:select.option>
    <flux:select.option>Option 4</flux:select.option>
</flux:select>
```

---

## Modal Patterns

```blade
{{-- Trigger --}}
<flux:modal.trigger name="edit-user">
    <flux:button>Edit</flux:button>
</flux:modal.trigger>

{{-- Modal (prefer flyout) --}}
<flux:modal name="edit-user" variant="flyout">
    <div class="space-y-6">
        <flux:heading size="lg">Edit User</flux:heading>
        <flux:input label="Name" wire:model="name" />
        <div class="flex gap-2">
            <flux:spacer />
            <flux:button type="submit" variant="primary">Save</flux:button>
        </div>
    </div>
</flux:modal>

{{-- In loops, use dynamic names --}}
@foreach ($users as $user)
    <flux:modal :name="'edit-'.$user->id" variant="flyout">...</flux:modal>
@endforeach
```

Opening/closing from Livewire:
```php
use Flux\Flux;

Flux::modal('edit-user')->show();
Flux::modal('edit-user')->close();
```

---

## Icons

Icons come from Heroicons - you can get the list of valid icons from [heroicon-list.txt](heroicon-list.txt):

```blade
<flux:button icon="plus">Add</flux:button>
<flux:button icon:trailing="chevron-down">Menu</flux:button>
<flux:icon.bolt />
<flux:icon name="bolt" />  {{-- dynamic --}}
```

---

## Key Reminders

1. **Use Flux components** - `<flux:text>` not `<p>`, `<flux:link>` not `<a>`
2. **You handle spacing** - use Tailwind gap/margin between components (prefer 6 for vertical spacing, 4 for horizontal, and 2 for grouped items)
3. **Don't over-style** - Flux handles visual styling, avoid adding color classes
4. **Button restraint** - most buttons are unstyled, only primary action gets `variant="primary"`. There is no need to use other variants unless specifically asked by the user.
5. **Flyout modals** - use `variant="flyout"` by default
6. **wire:key in loops** - always add for Livewire compatibility
7. **Consult docs** - when unsure or trying to use a component you haven't read recently, use the laravel boost `search-docs` tool before guessing - it will save the user time and money
8. **Icon names** - If you are not 100% about a name - consult the list.  Invalid names will cause flux to crash.

