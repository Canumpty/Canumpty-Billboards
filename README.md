# Canumpty Billboards

**Version 1.0.0**  
Developed by **Canumpty Digital Studios**

Canumpty Billboards is an owner-managed FiveM system for creating and maintaining persistent, double-sided advertising billboards. Each billboard has separate Side A and Side B artwork, placement controls, visibility controls, saved drafts, editing and deletion confirmation.

The public release includes the custom double-sided billboard model and clean Canumpty placeholder artwork. It contains **no saved billboard records, uploaded artwork, player identifiers, webhooks or private server settings**.

## Requirements

- FiveM server with OneSync.
- `qb-core`.
- `oxmysql` with a MySQL/MariaDB database connection.
- A MySQL server whose `max_allowed_packet` is at least 16 MB (the resource checks this at startup for image storage).

The required Node image/database libraries are included with this resource. No external image-host, Discord webhook or Redline resource is required.

## Installation

1. Extract `canumpty_billboards` into your server's `resources` folder.
2. Import [`install/schema.sql`](install/schema.sql) into the same database configured for oxmysql. It creates only these new tables:
   - `canumpty_billboards_records`
   - `canumpty_billboards_media`
   - `canumpty_billboards_migrations`
3. Add the resource after its dependencies in `server.cfg`:

```cfg
ensure oxmysql
ensure qb-core
ensure canumpty_billboards
```

4. Grant trusted staff the ACE permission shown in `owner.lua`:

```cfg
add_ace group.admin canumpty.billboards.admin allow
# Example only: add_principal identifier.license:YOUR_LICENSE_HERE group.admin
```

5. Start or restart the resource.

A ready server prints a management-ready message. If it reports that the database schema is missing, import `install/schema.sql`; do not point this public copy at another billboard resource's tables.

## Opening the manager

Only players granted `canumpty.billboards.admin` may use:

```text
/billboards
```

Diagnostic command for authorised staff:

```text
/billboardscheck
```

It reports saved/visible/draft counts in chat and detailed information in F8.

## Creating a billboard

1. Run `/billboards`.
2. Enter a name.
3. Choose an image for Side A and Side B. PNG, JPEG and WebP are supported; the source limit is 10 MB and 8 megapixels.
4. Press **Begin Placement** once both sides are ready.
5. Position the solid double-sided preview and press Enter to save it.

Images are normalised to PNG at 2600 × 1160 (65:29). Different aspect ratios receive black padding; they are not stretched or cropped. **Restore placeholder image** restores one of the generic public placeholders, not an image from the original server.

## Placement controls

During placement your character is held in place and a placement camera opens.

| Control | Action |
|---|---|
| `W` / `S` | Move billboard forward / backward |
| `A` / `D` | Move billboard left / right |
| `Q` / `E` | Raise / lower billboard |
| Left / Right arrow | Rotate billboard |
| Left `Alt` | Snap billboard to ground |
| Hold `Shift` | Fine movement, fine rotation and fine camera zoom |
| Mouse movement | Orbit the placement camera |
| Mouse wheel | Zoom the placement camera |
| `Enter` | Confirm position and save |
| `Escape` | Cancel placement and return to the manager |

When artwork is still finishing its save, pressing Enter confirms the position and lets you continue playing; the billboard appears once the artwork has finished safely saving.

## Managing existing billboards

Use the **Existing Billboards** tab to:

- Search by billboard name.
- Filter all, visible and hidden boards.
- Edit Side A or Side B independently.
- Edit placement.
- Teleport to an already placed billboard.
- Hide/show a placed billboard.
- Delete a billboard after the confirmation prompt.

Deleting a billboard removes only that billboard record. Artwork may remain in that server's own media library for use by another billboard.

## Image import and security

Local file upload works immediately. URL import is intentionally disabled by default for safety: `owner.lua` has an empty `ImageHosts` list. If a server owner wants URL imports, they must add only explicit, trusted HTTPS hostnames to that list.

## Configuration

- `config.lua` controls model, render distance, preview limits and the generic placeholder images.
- `owner.lua` controls the ACE permission, database use and image size/count limits.
- The included double-sided model uses the technical internal model name `legacy_trial_double_01`; do not rename the `.ydr`/`.ytyp` files unless their internal model references are rebuilt too.

## Licence

Released under the MIT Licence. See [LICENSE](LICENSE).

