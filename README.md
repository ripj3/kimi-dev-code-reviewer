# kimi-dev-code-reviewer
Automated GitHub PR reviews powered by Moonshot Kimi-Dev-72B. Whenever someone pushes to a pull-request, the workflow sends the patch to Kimi-Dev, then posts the model’s feedback back on the PR.
🤖 Kimi-Dev 72B Auto-Reviewer
Automated GitHub PR reviews powered by Moonshot Kimi-Dev-72B.
Whenever someone pushes to a pull-request, the workflow sends the patch to Kimi-Dev, then posts the model’s feedback back on the PR.

🗂 What’s inside
File	Purpose
.github/workflows/kimi-review.yml	GitHub Actions workflow that does the magic.
README.md (this file)	Quick-start, how it works, and tweak tips.

⚡ Quick start
Fork / clone this repo (or copy the workflow into your own).

Create a Moonshot API key
Open https://platform.moonshot.ai/ ► your project ► API Keys ► Generate Key ► copy the sk-… value.

Add the key as a repo secret

yaml
Copy
Edit
Settings ▸ Secrets ▸ Actions ▸ New repository secret
Name   : MOONSHOT_AI_KIMIDEV
Value  : sk-xxxxxxxxxxxxxxxxxxxxxxxx
Commit → open / update a pull-request.
In a few seconds you’ll see a comment titled “🤖 Kimi-Dev review” with line-referenced feedback.

That’s it—no other setup.

💸 Cost
The workflow calls Moonshot’s paid API. You pay only for the tokens Kimi generates:

Model slug (put in MODEL= line)	Context	Price / 1 K input tok	Price / 1 K output tok
kimidev-72b-chat (default)	8 K tok	$0.45	$1.35
kimidev-72b-32k	32 K tok	$0.55	$1.65
kimidev-72b-128k	128 K tok	$0.80	$2.40

Free trial credits: New Moonshot accounts usually get ¥50 (~$7) of credit.

🔧 Configuration
All tweaks live inside kimi-review.yml:

Variable / line	What it does	Example change
MODEL="kimidev-72b-chat"	Which model to call. Run curl https://api.moonshot.ai/v1/models to list what your key can access.	Switch to a longer-context variant.
max_tokens: 512	Max length of the AI response.	Increase for more detailed reviews.
temperature: 0.2	Higher = more creative, lower = more deterministic.	0.15 for stricter style.
System prompt	The instruction the bot follows.	Customize tone or required checklist items.

🛠 How it works (under the hood)
checkout – pulls the code so the job can fetch the diff.

curl diff_url – downloads the exact patch for this PR.

Payload → Moonshot – sends the diff as user content in an OpenAI-compatible /chat/completions call.

Parse JSON – extracts choices[0].message.content.

Comments API – POSTs that text back to the PR via GitHub REST.

No external services, proxies, or containers—just the official Moonshot endpoint.

🧪 Troubleshooting
Symptom	Fix
Workflow fails “401 Unauthorized”	Key pasted with spaces or expired → generate a new key, re-save secret.
“404 Model not found”	Change MODEL= to a slug that appears in the /v1/models list for your key.
Comment contains Chinese placeholders	That’s Kimi’s style for unknown context; raise max_tokens or add more diff context.
No comment posted	Ensure the job has pull-requests: write permission (present by default).

📜 License
Workflow and README are MIT-licensed.
Kimi-Dev-72B weights and Moonshot API are released by Moonshot AI under their respective terms—see https://moonshot.ai for details.

🙌 Credits
Ripj3 - created as a result of frustrationt trying to integrate into other IDKs Inspired by the Moonshot AI community and the broader OSS PR-review bot ecosystem.
Feel free to open issues or PRs to improve prompts, error handling, or add multi-file inline review support.
