# Updating an existing AAP with a new AMI

AWS rotates the public Windows Server base AMIs roughly monthly and **deregisters**
the old IDs. When that happens, the demo's `RunInstances` call fails with:

```
An error occurred (AuthFailure) when calling the RunInstances operation:
Not authorized for images: [ami-XXXXXXXXXXXXXXXXX]
```

This runbook gets a fresh AMI into an already-running AAP so the demo works again.

## 1. Find the current AMI

The source of truth is the AWS SSM public parameter (region matters — the demo uses
`us-west-1`, set in `roles/vm/vars/main.yml`):

```bash
aws ssm get-parameters \
  --names /aws/service/ami-windows-latest/Windows_Server-2025-English-Full-Base \
  --region us-west-1 \
  --query 'Parameters[0].Value' --output text
```

Optionally confirm it is available:

```bash
aws ec2 describe-images --image-ids <ami-id> --region us-west-1 \
  --query 'Images[0].{Name:Name,State:State,Public:Public}' --output table
```

> Needs valid AWS credentials. The laptop's `~/.aws/credentials` uses a static IAM
> key — if it returns `security token ... is invalid`, the key was rotated/disabled;
> generate a new one. (This identity is separate from the demo's in-AAP
> `service.ansible` account.)

## 2. Update the pinned AMI

Edit `roles/vm/vars/main.yml`:

```yaml
# Microsoft Windows Server 2025 Base (<name>, us-west-1)
vm_image: <new-ami-id>
```

Commit, push, and merge to the branch the AAP **project** tracks (default `main`).

## 3. Sync the AAP project

The AAP **project** `aap.dailydemo.windows` pulls playbooks/roles from GitHub. The new
`vm_image` is only picked up after a project sync:

- AAP UI: **Automation Execution → Projects → `aap.dailydemo.windows` → Sync**, or
- If the project has **Update Revision on Launch** enabled, launching any template
  syncs first automatically.

## 4. Re-run / test

- Clean up any half-built run first: launch **`Delete Daily Demo Windows`**.
- Then launch the **Windows Daily Demo workflow**, or test just the VM step with the
  **`DDW - VM Create`** job template (`playbooks/02_create_instance.yml` → `vm` role).

## 5. Verify

- The `DDW - VM Create` job completes without the `AuthFailure` error.
- The success ServiceNow ticket reports the new `AWS AMI ID` (`my_ami_id`).
- An EC2 instance is running in `us-west-1` from the new AMI.

## 6. Document

- Add a `CHANGELOG.md` entry under `### Fixed`.
- Open a GitHub issue referencing the ServiceNow incident number.
