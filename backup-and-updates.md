# Backup and Updates

Chapter 2's promise, kept: Open WebUI
is its data directory. This chapter is
the care and feeding of that fact.

## Backup

To back up Open WebUI:

1. **Stop the server** (or accept
   copying a live database — SQLite
   tolerates this better than most,
   but a stopped copy is a clean one).
2. **Copy the whole data directory**:
   the `webui.db` database, the
   `uploads/` and `cache/` folders,
   and the vector database that holds
   your documents' embeddings
   (`vector_db/`). Docker route: the
   named volume is that directory, and
   `docker run --rm -v open-webui:/data
   -v $PWD:/backup busybox tar czf
   /backup/open-webui-backup.tgz -C /
   data` is the standard shape.
3. **Write down, beside the backup**:
   the version you backed up (`pip show
   open-webui` on a pip install, or the
   `version` field of `GET /api/config`),
   and your `WEBUI_SECRET_KEY`. A backup without
   the key restores the data and logs
   everybody out anyway; a backup
   without the version is a riddle
   for restore day.

Restoring is the same film backwards:
stop, put the directory back, start
the same version (or a newer one —
upgrades migrate the database forward
on start; downgrades do not promise
to migrate it back, which is reason
two to keep the old backup until the
new version has behaved for a week).

What a backup does *not* contain:
the models. Backends keep their own
(chapter 6's Ollama has its own model
store, and its own backup story in
its own guide). Your backup restores
the front door, the accounts and every
conversation; the models are re-pulled
or reconnected on the far side.

Check: a backup you have never
restored is a hope, not a backup.
Once, on a spare folder or a spare
port, restore your copy and sign in.
It takes ten minutes and converts
this chapter from advice into
knowledge.

## Updates

Open WebUI releases often — weekly is
a fair description — and the project
moves settings and defaults between
versions more casually than a
household utility ideally would.
Working rules:

- **Docker route:** pull the new
  image, recreate the container, keep
  the volume. Pin a version tag
  (`:0.9.5`-style) instead of `:main`
  once the install matters to people;
  update on your schedule, not the
  registry's.
- **Pip route:** `pip install -U
  open-webui` inside the same venv,
  after reading the release notes and
  after a backup. The dependency tree
  is large; expect updates to take
  minutes, and expect the occasional
  version to want a different Python
  (chapter 3's warning) — that is a
  rebuild-the-venv afternoon, not a
  five-minute job, and another reason
  the keep-it route is Docker.
- **Read the log after starting.**
  Migrations announce themselves
  there; so do deprecated settings,
  which Open WebUI mostly tolerates
  and occasionally retires.
- **Hold still before big days.** An
  install that a household depends on
  does not get updated the night
  before it's needed. This rule is
  free and will pay for this whole
  guide the first time it saves you.
