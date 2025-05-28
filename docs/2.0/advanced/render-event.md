# View Render Event

[[TOC]]

## Introduction

The `view_render_event()` function in Krayin allows developers to inject content dynamically before or after the main content of a template. This functionality is useful for modifying template output without directly altering the template file itself, enhancing flexibility and maintainability in your application.

## Render View

To utilize the `view_render_event()` function effectively, follow these steps:

In this example, `admin.contacts.persons.edit.form_controls.before` and `admin.contacts.persons.edit.form_controls.after`
are custom event names that denote where content should be injected before and after the form controls section, respectively inside the `packages/Webkul/Admin/src/Resources/views/contacts/persons/edit.blade.php` file.


### Listening to Events

To handle these events and inject content dynamically, you need to listen to them in your application’s event system (typically in the `EventServiceProvider`).

Create a file named `EventServiceProvider.php` inside your package’s `Providers` directory:

In the `boot()` method of your `EventServiceProvider`, add event listeners as follows:

```php
<?php

namespace Webkul\<PackageName>\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Event;

class EventServiceProvider extends ServiceProvider
{
    /**
     * Register any events for your application.
     *
     * @return void
     */
    public function boot()
    {
        Event::listen('admin.contacts.persons.edit.form_controls.before', function($viewRenderEventManager) {
            $viewRenderEventManager->addTemplate('product::shop.quotes.create');
        });

        Event::listen('admin.contacts.persons.edit.form_controls.after', function($viewRenderEventManager) {
            $viewRenderEventManager->addTemplate('product::shop.quotes.view');
        });
    }
}
```

In this example:

- We’re listening to the event: `admin.contacts.persons.edit.form_controls.before` amd `admin.contacts.persons.edit.form_controls.after`.
- We're dynamically injecting the template: `product::shop.quotes.create` and `product::shop.quotes.view`, where `product` is the supposed namespace of the package and `shop.quotes.create` and `shop.quotes.view` are the view names.
- The contents of that Blade file will be rendered at the position of the event in the DOM

:::warning
   Make sure that you have registered the **`EventServiceProvider`** in your own service provider.
:::

### Implementation Details

- `$viewRenderEventManager->addTemplate()`: This method adds the specified template file to the rendering queue for the corresponding event. When the event is triggered during template rendering, Krayin will include the specified template's content at the designated injection point.

- `Event Handling`: Ensure that you properly handle the events within your application’s event flow. This involves registering listeners correctly in EventServiceProvider and ensuring that the templates being injected are structured and formatted according to your application's requirements.

### Considerations

- `Integration`: Integrate this functionality carefully into your Krayin application to maintain coherence and readability of your codebase

- `Customization`: Customize event names (`'admin.contacts.persons.edit.form_controls.before'`, `'admin.contacts.persons.edit.form_controls.after'`) and template paths according to your specific application needs and structure.

By following these steps, you can effectively leverage the `view_render_event()` function in Krayin to dynamically inject content into template sections, enhancing flexibility and customization options within your application.