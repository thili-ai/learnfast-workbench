# Learn Fast Workbench

**Your lab bench for a Learn Fast internship.** One command, and you have every library the drill needs —
no `pip install` roulette, no CUDA, no version archaeology.

The workbench is **empty on purpose**. It contains the tools, not the answers. You write your own
notebooks in `work/`, and that folder becomes the repo you submit at the end of the drill.

---

## Start here

**1. Click "Use this template"** (green button, top of this page) → *Create a new repository*.
Name it whatever you like — `learnfast-work` is a good default. **This repo is now yours.**

> Use the template button, not Fork. A fork carries our commit history; a template gives you a clean
> first commit, so the work in it is unambiguously yours — which matters when your drill is graded.

**2. Clone it and start the workbench for your drill:**

```bash
git clone https://github.com/<your-username>/learnfast-work.git
cd learnfast-work
docker compose -f lfvision/docker-compose.yml up
```

**3. Open** <http://127.0.0.1:8889/?token=learnfast>

**4. Work in the `work/` folder.** Create notebooks, write code, break things. Everything you save there
is on your own machine, in your own git repo.

**5. When the drill is done:**

```bash
git add work/ && git commit -m "my drill build" && git push
```

Submit that repository's URL on the drill page. Done.

To stop the workbench: `Ctrl-C`, or `docker compose -f lfvision/docker-compose.yml down`.

---

## Which workbench do I need?

Pick the folder matching your drill's track. Each one is a separate environment — start only the one
you need.

| Folder | Drills | API keys needed |
|---|---|---|
| `lfvision/` | CNNs, GANs, Quantization, Vision Transformers | **None** |

*(More tracks land here as they're published.)*

---

## Things worth knowing

**Give Docker enough memory.** Docker Desktop defaults to about 2 GB. Training anything real will be
killed mid-run and look like a broken image. Set **8 GB or more** in Docker Desktop → Settings →
Resources. This is the single most common "it doesn't work".

**The first start is slow.** It downloads a multi-gigabyte image once. After that, seconds.

**Your files live on your machine.** `work/` is a mounted folder — the container reads and writes your
real directory. Delete the container, your work stays. But anything saved *outside* `work/` inside the
container is gone when it stops.

**No GPU needed, ever.** These images install CPU-only PyTorch deliberately. The drills are built around
small models so the internals stay visible on a laptop.

**Nothing leaves your machine.** The vision workbench needs no API keys and makes no outbound calls
except downloading model weights when you ask it to.

---

## Troubleshooting

| What you see | What's happening |
|---|---|
| `port is already allocated` | Something else uses 8889. Change the left-hand number in `docker-compose.yml` to `8890:8888`. |
| Kernel dies during training | Docker memory limit — raise it to 8 GB+ (above). |
| `no matching manifest` | Your machine's architecture isn't in the published image; the compose file falls back to building locally, which takes a while but works. |
| Notebook asks for a token | It's `learnfast`, or use the full URL above. |

---

## What's in here

```
lfvision/
  Dockerfile          how the environment is built
  requirements.txt    the libraries, trimmed to what the drills actually use
  docker-compose.yml  how it starts
work/                 ← your notebooks go here (this is what you submit)
```

You're welcome to read and change any of it — it's your repo now. Adding a library is a line in
`requirements.txt` plus `docker compose -f <track>/docker-compose.yml build`.

---

MIT licensed. Built for [Learn Fast](https://learnfast.thili.ai) internships by [thili.ai](https://thili.ai).
