# tspl2
Library for handling with thermal label printers, controlled by TSPL/TSPL2 language

# Examples

## Rust
See: [simple.rs](examples/simple.rs)
```rust
use anyhow::Result;
use tspl2::{Alignment, Barcode, Font, HumanReadable, NarrowWide, Printer, Rotation, Size, Tape};

fn main() -> Result<()> {
    // Get access to the printer
    std::process::Command::new("sudo")
        .args(["chmod", "777", "/dev/usb/lp0"])
        .output()?;

    let tape = Tape {
        width: Size::Metric(30.0),
        height: Some(Size::Metric(20.0)),
        gap: Size::Metric(2.0),
        gap_offset: None,
    };

    let mut printer = Printer::with_resolution("/dev/usb/lp0", tape, 300)?;
    printer
        .cls()?
        .qrcode(
            Size::Metric(9.0),
            Size::Metric(0.0),
            35,
            6,
            Rotation::NoRotation,
            None,
            "0123456789AB",
        )?
        .text(
            Size::Metric(15.0),
            Size::Metric(14.5),
            Font::Font24x32,
            Rotation::NoRotation,
            1,
            1,
            Some(Alignment::Center),
            "0123456789AB",
        )?
        .print(1, None)?;

    printer
        .cls()?
        .barcode(
            Size::Metric(15.0),
            Size::Metric(0.0),
            Barcode::Barcode39,
            Size::Metric(15.0),
            HumanReadable::ReadableAlignsToCenter,
            Rotation::NoRotation,
            NarrowWide::N1W3,
            Some(Alignment::Center),
            "0123456789AB",
        )?
        .print(1, None)?;

    Ok(())
}
```

## for Windows
Using printer name for Printer::with_resolution, for TSC TE344, with below code to create printer instance.
```rust
use anyhow::Result;
use tspl2::{Alignment, Barcode, Font, HumanReadable, NarrowWide, Printer, Rotation, Size, Tape};

fn main() -> Result<()> {
    let tape = Tape {
        width: Size::Metric(30.0),
        height: Some(Size::Metric(20.0)),
        gap: Size::Metric(2.0),
        gap_offset: None,
    };

    let mut printer = Printer::with_resolution("TSC TE344", tape, 300)?;
    // same with previous example to print 
    ...
    Ok(())
}
```
