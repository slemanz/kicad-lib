## Third-party content

Some footprints and 3D models in this repository were copied from the
official KiCad Libraries (https://gitlab.com/kicad/libraries), maintained
by the KiCad Library Team.

Those files are licensed under CC-BY-SA 4.0 with the KiCad library
exception, and remain under that license here. See LICENSE-CC-BY-SA-4.0.

Vendored from KiCad 9.0.x. Files retain their upstream names.

Two of those 3D models were **repositioned, not redrawn**:
`3d/ESP-01-SOCKET.step` and `3d/WC1602A-SOCKET.step` are
`PinSocket_2x04_P2.54mm_Vertical.step` and `PinSocket_1x16_P2.54mm_Vertical.step`
rotated 90 degrees about Z, so pin 1 lands where the module footprint puts it
per LIBRARY_CONVENTION.md section 5.2. The geometry is otherwise untouched.

## ESP-01 3D model

`3d/ESP-01.step` was supplied by the repository owner from a third-party CAD
model of the Ai-Thinker ESP-01, not by the KiCad libraries, which ship an
`ESP-01` footprint whose 3D model is missing. It was rotated and translated onto
the footprint origin at its socketed height per LIBRARY_CONVENTION.md section
5.2, and is otherwise the geometry as downloaded. Confirm the original model's
licence before redistributing this repository.

## Component Search Engine

`3d/SMD_U254-051N-4BH806.step` was downloaded from Component Search Engine
(https://componentsearchengine.com), operated by SamacSys, and is redistributed
here under their terms of use for CAD models. The geometry is unmodified, the
file was only renamed to match its footprint per LIBRARY_CONVENTION.md section 5.

The symbol and footprint for that part are **not** from the same package. Both
were drawn here from the XKB mechanical drawing, because the bundled footprint
substitutes round holes for the four oblong shell slots the datasheet specifies.
