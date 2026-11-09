# Privacy and sharing

These assistants are built from your real life, so they collect sensitive details quickly. Decide up front what goes in.

## Keep out of anything you share or publish to GitHub

- **Vehicle VIN and license plate.** A VIN plus your name is enough for some scams and lets anyone pull your car's history.
- **Full names of other people** (partner, roommates, family, your mechanic). Use roles: "guest", "roommate", "shop A".
- **Home address, dorm and room numbers**, photos that show them (mail, lease, ID cards, window views of your street).
- **Body measurements and weight** unless you're fine with them being public.
- **Receipts as images.** They often show card digits, phone numbers and addresses. Have Claude extract the fields you need and don't keep the image.

That's why this repo has a template and prompts instead of my actual three pages.

## How sharing works for Claude Artifacts

- A new Artifact is **private** to you. You choose to share it from its Share menu.
- "Anyone with the link" means anyone who gets the link can open it, including the data the page shows. Two of my three assistants were set to link-sharing; that's a choice to revisit if the data is personal.
- The `db` capability lets you set access rules per collection. Without rules, signed-in viewers can read the shared data. The room assistant sets every collection to editor-only (`read: "admin", write: "admin"`), so link viewers see the page but not the records. Ask Claude to do the same for anything personal: *"make every collection readable and writable only by people who can edit this page."*
- Data under `data/users/<your id>/` is private to you even on a shared page.
- The "Ask Claude" box (`sample`) sends the page's data to Claude for that one answer and is billed to whoever clicks it.

## Before you publish your own repo

Run this once in the repo folder and read every hit:

```bash
grep -rniE "vin|plate|address|phone|@|[0-9]{3}-[0-9]{3}-[0-9]{4}" .
```
