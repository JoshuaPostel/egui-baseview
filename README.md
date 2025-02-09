# egui-baseview
![Test](https://github.com/BillyDM/egui-baseview/workflows/Rust/badge.svg)
[![Crates.io](https://img.shields.io/crates/v/egui-baseview.svg)](https://crates.io/crates/egui-baseview)
[![License](https://img.shields.io/crates/l/egui-baseview.svg)](https://github.com/BillyDM/egui-baseview/blob/main/LICENSE)

A [`baseview`] backend for [`egui`].

<div align="center">
    <img src="screenshot.png">
</div>

## Simple Usage Example

```rust
use baseview::{Size, WindowOpenOptions, WindowScalePolicy};
use egui::Context;
use egui_baseview::{EguiWindow, Queue, RenderSettings, Settings};

fn main() {
    let settings = Settings {
        window: WindowOpenOptions {
            title: String::from("egui-baseview simple demo"),
            size: Size::new(400.0, 200.0),
            scale: WindowScalePolicy::SystemScaleFactor,
        },
        render_settings: RenderSettings::default(),
    };

    let state = State::new();

    EguiWindow::open_blocking(
        settings,
        state,
        // Called once before the first frame. Allows you to do setup code and to
        // call `ctx.set_fonts()`. Optional.
        |_egui_ctx: &Context, _queue: &mut Queue, _state: &mut State| {},
        // Called before each frame. Here you should update the state of your
        // application and build the UI.
        |egui_ctx: &Context, _queue: &mut Queue, state: &mut State| {
            egui::Window::new("egui-baseview simple demo").show(&egui_ctx, |ui| {
                ui.heading("My Egui Application");
                ui.horizontal(|ui| {
                    ui.label("Your name: ");
                    ui.text_edit_singleline(&mut state.name);
                });
                ui.add(egui::Slider::u32(&mut state.age, 0..=120).text("age"));
                if ui.button("Click each year").clicked {
                    state.age += 1;
                }
                ui.label(format!("Hello '{}', age {}", state.name, state.age));
            });
        },
    );
}

struct State {
    pub name: String,
    pub age: u32,
}

impl State {
    pub fn new() -> State {
        State {
            name: String::from(""),
            age: 30,
        }
    }
}
```

## VST / LV2 / AU Plugins

Examples of how to use this library for audio plugins can be found here:
* [`egui_baseview_test_vst2`]

[`baseview`]: https://github.com/RustAudio/baseview
[`egui`]: https://github.com/emilk/egui
[`egui_baseview_test_vst2`]: https://github.com/DGriffin91/egui_baseview_test_vst2
