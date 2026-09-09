# Hack the North 2026 x Baseten

Hey hackers!

We're excited to meet everyone at Hack the North this weekend. This guide covers the prizes,
credits, hosted models, coding tools, and H100 training access available through Baseten during
the event.

## What is Baseten?

[Baseten](https://www.baseten.co/) is an inference-focused AI infrastructure platform used by
AI companies including Descript, Bland, Clay, Writer, and OpenEvidence.

For this hackathon, you can use Baseten in three ways:

- Call hosted open-source models such as DeepSeek, Kimi, GLM, Inkling, GPT-OSS, and Nemotron
  through a single OpenAI-compatible API.
- Use H100 GPU workstations to train or fine-tune your own models.
- Route Claude Code, Codex CLI, or Pi through Baseten-hosted models with Baseten Switch.

You do not need to manage inference servers or GPU infrastructure. Start with the hosted Model
APIs for the quickest path to a demo, use Baseten Switch to power your coding agent, or visit the
Baseten booth if your project needs training compute.

## Prizes

### Grand Prize

- A free trip to San Francisco to meet the Baseten team
- Final-round interviews with Baseten
- $200 in Baseten credits
- A signed hardcover copy of *Inference Engineering*

### Runner-Up

- $100 in Baseten credits
- AirPods 4
- A signed hardcover copy of *Inference Engineering*

## Credits

Every hacker can claim free Baseten credits with promo code shared in the event Slack! Check `#spons-baseten-2026` for more details!

To redeem the code:

1. [Create a Baseten account](https://app.baseten.co/).
2. In the Baseten dashboard, select **Billing and usage** in the left sidebar.
3. Scroll to **Redeem promo credits**.
4. Enter the promo code and redeem it.

Credits apply to the workspace, not each individual member. If your team shares one Baseten
workspace, the person who created that workspace must redeem the code. You only need to redeem it
once for the whole team.

## Inference

[Model APIs](https://docs.baseten.co/inference/model-apis/overview) let you call hosted LLMs with
the OpenAI Chat Completions API or the Anthropic Messages API. Baseten manages the model deployment
and serving infrastructure.

### Get started

1. [Create an API key](https://app.baseten.co/settings/api_keys).
2. Save it in your shell. Do not commit it to Git.
3. Install the OpenAI Python SDK.
4. Point the SDK at Baseten and choose a model from the live catalog.

```bash
export BASETEN_API_KEY="your-api-key"
pip install openai
```

```python
import os

from openai import OpenAI

client = OpenAI(
    api_key=os.environ["BASETEN_API_KEY"],
    base_url="https://inference.baseten.co/v1",
)

response = client.chat.completions.create(
    model="zai-org/GLM-5.3",
    messages=[
        {"role": "user", "content": "Pitch me a hackathon project."},
    ],
)

print(response.choices[0].message.content)
```

The API also supports streaming, tool calling, structured outputs, JSON mode, reasoning, vision,
and audio. Support varies by model, so check the live documentation before choosing one.

Useful links:

- [Model APIs overview and quickstart](https://docs.baseten.co/inference/model-apis/overview)
- [Current models, features, context windows, and model slugs](https://docs.baseten.co/inference/model-apis/overview#supported-models)
- [Pricing, rate limits, and prompt caching](https://docs.baseten.co/inference/model-apis/pricing-and-limits)
- [Tool calling](https://docs.baseten.co/inference/function-calling)
- [Structured outputs](https://docs.baseten.co/inference/structured-outputs)
- [Reasoning](https://docs.baseten.co/inference/model-apis/reasoning)
- [Vision](https://docs.baseten.co/inference/model-apis/vision)
- [Audio](https://docs.baseten.co/inference/model-apis/audio)

You can also query the live model catalog from your terminal:

```bash
curl https://inference.baseten.co/v1/models \
  -H "Authorization: Bearer $BASETEN_API_KEY"
```

## Rate Limits

If you receive an HTTP `429` response, your workspace has reached a request or token limit. Check
the [current Model APIs limits](https://docs.baseten.co/inference/model-apis/pricing-and-limits),
then submit the event rate-limit form shared in the attendee channel.

While you wait:

- Retry `429` responses with exponential backoff.
- Avoid retrying immediately in a tight loop.
- Run independent requests concurrently, but keep the total request rate below your workspace
  limit.
- Keep shared instructions and examples at the start of prompts so automatic prompt caching can
  reuse them.

If the form does not resolve the issue, visit the Baseten booth or ask in
`#spons-baseten-2026`.

## Error Codes

| Code | Meaning | What to do |
| ---: | --- | --- |
| `400` | Invalid request | Check the model slug, request body, and model-specific parameters. |
| `401` | Invalid or missing API key | Confirm `BASETEN_API_KEY` is set and sent as a bearer token. |
| `402` | Payment required | Redeem the event credits or check your workspace billing status. |
| `404` | Model not found | Check the model slug against the live model catalog. |
| `429` | Rate limit exceeded | Retry with exponential backoff and use the event rate-limit form if needed. |
| `500` | Internal server error | Retry the request. Ask for help if the error continues. |

See the [inference troubleshooting guide](https://docs.baseten.co/troubleshooting/inference) for
more debugging steps.

## Training

We are offering H100 workstations for hackers who want to train or fine-tune their own models.
Visit the Baseten booth first so the team can enable training access and help you choose an
appropriate setup.

Once your workspace has access, the usual workflow is:

1. Install the Baseten CLI and sign in.
2. Define your training image, GPU requirements, commands, secrets, cache, and checkpoint storage.
3. Submit the job and follow its logs and metrics from the CLI or dashboard.
4. Save outputs under the Baseten checkpoint directory so they persist after the job stops.
5. Deploy a completed checkpoint when you are ready to test inference.

Start here:

- [Training overview](https://docs.baseten.co/training)
- [Train and deploy your first model](https://docs.baseten.co/training/getting-started)
- [Create an interactive GPU workstation](https://docs.baseten.co/reference/cli/baseten/train-workstation)
- [Connect to a training environment](https://docs.baseten.co/training/remote-access)
- [Monitor and manage training jobs](https://docs.baseten.co/training/management)
- [Training SDK reference](https://docs.baseten.co/reference/sdk/training)
- [Baseten ML Cookbook examples](https://github.com/basetenlabs/ml-cookbook)

## Baseten Switch

[Baseten Switch](https://github.com/basetenlabs/baseten-switch) is a local macOS app and gateway
that routes supported AI coding tools through models hosted on Baseten. One global switch controls
Baseten routing, while per-client mappings let you choose the model used by Claude Code, Codex CLI,
or Pi.

Baseten-routed requests use the workspace selected by your Baseten CLI profile. **If you redeemed
hackathon inference credits in that workspace, you can use those credits for coding-agent traffic
through Baseten Switch too.**

Install and connect Claude Code:

```bash
brew install basetenlabs/baseten/baseten-switch
baseten-switch setup
baseten-switch up --install
baseten-switch claude on
baseten-switch doctor --probe
```

`setup` checks your Baseten CLI login and creates the initial configuration. `up --install`
starts the local gateway and installs the menu bar app. Restart Claude Code after enabling the
integration so new sessions use the configured route.

Codex support is opt-in:

```bash
baseten-switch codex on
baseten-switch codex route zai-org/GLM-5.2
codex --profile baseten
```

Baseten Switch currently supports macOS 13 or newer on Apple Silicon and Intel. It is in beta, and
the app is not yet notarized by Apple. If macOS blocks the first launch, approve it under
**System Settings > Privacy & Security**. See the
[official setup and troubleshooting guide](https://github.com/basetenlabs/baseten-switch#quick-start)
for routing controls, upgrades, logs, and uninstall instructions.

## Getting Help

- Read the [Baseten documentation](https://docs.baseten.co/). Its built-in docs assistant can
  answer questions about the current APIs.
- Ask in `#spons-baseten-2026` so the Baseten team can respond and other hackers can benefit from
  the answer.
- Visit the Baseten booth for training access, rate-limit problems, architecture advice, or help
  debugging your project.

Bring the request ID and full error response when asking for API help. For training issues, bring
the training job ID and the relevant log lines.

Good luck, and come show us what you build!
