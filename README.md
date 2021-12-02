# **Allstar**

## Overview 

-  [What Is Allstar?](#what-is-allstar)

## Disabling Unwanted Issues

-  [Help! I'm getting issues created by Allstar and I don't want them!](#disabling-unwanted-issues) 

## Getting Started 

-  [Background](#background) 
-  [Pre-Installation Decisions](#pre-installation-decisions) 
-  [Installation Options](#installation-options)
    - [Quickstart Installation](#quickstart-installation)
    - [Manual Installation](#manual-installation)
 
[TODO: finish Table of Contents]   
________
________

## Overview 

### What is Allstar?

Allstar is a GitHub App installed on organizations or repositories to set and
enforce security policies. Its goal is to be able to continuously monitor and
detect any GitHub setting or repository file contents that may be risky or do
not follow security best practices. If Allstar finds a repository to be out of
compliance, it will take an action such as create an issue or restore security
settings.

The specific policies are intended to be highly configurable, to try to meet the
needs of different project communities and organizations. Also, developing and
contributing new policies is intended to be easy.

Allstar is developed under the [OpenSSF](https://openssf.org/) organization, as
a part of the [Securing Critical Projects Working
Group](https://github.com/ossf/wg-securing-critical-projects). The OpenSSF runs
[an instance of Allstar here](https://github.com/apps/allstar-app) for anyone to
install and use on their GitHub organizations. However, Allstar can be run by
anyone if need be, see [the operator docs](operator.md) for more details.

## Disabling Unwanted Issues
If you're getting unwanted issues created by Allstar, follow [these directions](opt-out.md) to opt out. 

## Getting Started

### Background

Allstar is highly configurable. There are three main levels of controls: 

- **Org level**: Organization administrators can choose to enable Allstar on: 
   -  all repositories in the org; 
   -  most repositories, except some that are opted out; 
   -  just a few repositories that are opted in. 

These configurations are done in the organization's .`allstar` repository.

- **Repo level:** Repository maintainers in an organization that uses
   Allstar can choose to opt their repository in or out of organization-level
   enforcements. Note: these repo-level controls are only functional when "repo
   override" is allowed in the org-level settings. These configurations are
   done in the repository's `.allstar` directory.

- **Policy level:** Administrators or maintainers can choose which policies
   are enabled on specific repos and which actions Allstar takes when a policy
   is violated. These configurations are done in a policy yaml file in either
   the organization's `.allstar` repository (admins), or the repository's
   `.allstar` directory (maintainers). 

### Pre-Installation Decisions

Before installing Allstar, you should decide approximately how many repositories
you want Allstar to run on. This will help you choose between the Opt-In and
Opt-Out strategies. 

-  The Opt In strategy allows you to manually add the repositories you'd
   like Allstar to run on. If you do not specify any repositories, Allstar will
   not run despite being installed. Choose the Opt In strategy if you want to enforce
   policies on only a small number of your total repositories, or want to try
   out Allstar on a single repository before enabling it on more. 

-  The Opt Out strategy (recommended) enables Allstar on all repositories
   and allows you to manually select the repositories to opt out of Allstar
   enforcements. You can also choose to opt out all public repos, or all
   private repos. Choose this option if you want to run Allstar on all
   repositories in an organization, or want to opt out only a small number of
   repositories or specific type (i.e., public vs. private) of repository.

<table>
<thead>
<tr>
<th></th>
<th><strong>Opt Out (Recommended)</strong><br>
<strong>optOutStrategy = true</strong></th>
<th><strong>Opt In</strong><br>
<strong>optOutStrategy = false</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Default behavior </td>
<td>All repos are enabled</td>
<td>No repos are enabled </td>
</tr>
<tr>
<td>Manually adding repositories</td>
<td>Manually adding repos disables Allstar on those repos</td>
<td>Manually adding repos enables Allstar on those repos</td>
</tr>
<tr>
<td>Additional configurations</td>
<td>optOutRepos: Allstar will be disabled on the listed repos<br>
<br>
optOutPrivateRepos: if true, Allstar will be disabled on all private repos
<br>
<br>
optOutPublicRepos: if true, Allstar will be disabled on all public
repos<br>
<br>
(optInRepos: this setting will be ignored)</td>
<td>optInRepos: Allstar will be enabled on the listed repos <br>
<br>
(optOutRepos: this setting will be ignored)</td>
</tr>
<tr>
<td>Repo Override </td>
<td>If true: Repos can opt out of their organization's Allstar enforcements
using the settings in their own repo file. Org level opt-in settings that
apply to that repository are ignored. <br>
<br>
If false: repos cannot opt out of Allstar enforcements as configured at the
org level. </td>
<td>If true: Repos can opt in to their organization's Allstar enforcements even
if they are not configured for the repo at the org level. Org level opt-out
settings that apply to that repository are ignored.<br>
<br>
If false: Repos cannot opt into Allstar enforcements if they are not
configured at the org level. </td>
</tr>
</tbody>
</table>

### Installation Options

Both the Quickstart and Manual Installation options involve installing the Allstar app. You may review the permissions requested. The app asks for read access to most settings and file contents to detect security compliance. It requests write access to issues to
create issues, and to checks to allow the `block` action.

#### Quickstart Installation 
This installation option will enable Allstar using the
Opt Out strategy on all repositories in your  organization. All current policies
will be enabled, and Allstar will alert you of
policy violations by filing an issue. This is the quickest and easiest way to start using Allstar, and you can still change any configurations later. 

Effort: very easy 

Steps:   
1) Install the [Allstar app](https://github.com/apps/allstar-app) (choose "All
Repositories" under Repository Access, even if you plan to disable Allstar on some repositories later)  
2) fork the [sample repository
](https://github.com/jeffmendoza/dot-allstar-quickstart)

That's it! All current Allstar policies [TODO: link]  are now enabled on all
your repositories. Allstar will create an issue if a policy is violated. 

To change any configurations (such as opting out of specific repositories,
disabling particular enforcements, or changing the actions taken in response to
policy violations), see directions [TODO:add].

#### Manual Installation
This installation option will walk you through creating
configuration files according to either the Opt In or Opt Out strategy. This
option provides more granular control over configurations right from the start.

Effort: moderate

Steps:   
1) install the [Allstar app](https://github.com/apps/allstar-app) (choose "All
Repositories" under Repository Access,  even if you don't plan to use Allstar on
all your repositories)  
2) Follow the [manual installation directions](manual-install.md) to create org-level or 
repository-level Allstar config files and individual policy files.  

### Secondary Org-Level configuration location

By default, org-level configuration files, such as the `allstar.yaml` file
above, are expected to be in a `.allstar` repository. If this repository does
not exist, then the `.github` repository `allstar` directory is used as a
secondary location. To clarify, for `allstar.yaml`:

| Prescedence | Repository | Path |
| - | - | - |
| Primary | `.allstar` | `allstar.yaml` |
| Secondary | `.github` | `allstar/allstar.yaml` |

This is also true for the org-level configuration files for the individual
policies, as described below.

### Repository Override

Individual repositories can also opt in or out using configuration files inside
those repositories. For example, if the organization is configured with the
opt-out strategy, a repository may opt itself out by including the file
`.allstar/allstar.yaml` with the contents:

```
optConfig:
  optOut: true
```

Conversely, this allows repositories to opt-in and enable Allstar when the
organization is configured with the opt-in strategy. Because opt-in is the
default strategy, this is how Allstar works if the `.allstar` repository doesn't
exist.

At the organization-level `allstar.yaml`, repository override may be disabled
with the setting:

```
optConfig:
  disableRepoOverride: true
```

This allows an organization-owner to have a central point of approval for
repositories to request an opt-out through a GitHub PR. Understandably, Allstar
or individual policies may not make sense for all repositories.

### Policy Enable

Each individual policy configuration file (see below) also contains the exact
same `optConfig` configuration object. This allows granularity to enable
policies on individual repositories. A policy will not take action unless
it is enabled **and** Allstar is enabled as a whole.

### Definition

- [Organization level enable configuration](https://pkg.go.dev/github.com/ossf/allstar@v0.0.0-20210728182754-005854d69ba7/pkg/config#OrgOptConfig)
- [Repository Override enable configuration]( https://pkg.go.dev/github.com/ossf/allstar@v0.0.0-20210728182754-005854d69ba7/pkg/config#RepoOptConfig)

## **Actions**

Each policy can be configured with an action that Allstar will take when it
detects a repository to be out of compliance.

- `log`: This is the default action, and actually takes place for all
  actions. All policy run results and details are logged. Logs are currently
  only visible to the app operator, plans to expose these are under discussion.
- `issue`: This action creates a GitHub issue. Only one issue is created per
  policy, and the text describes the details of the policy violation. If the
  issue is already open, it is pinged with a comment every 24 hours (not
  currently user configurable). Once the violation is addressed, the issue will
  be automatically closed by Allstar within 5-10 minutes.
- `fix`: This action is policy specific. The policy will make the changes to the
  GitHub settings to correct the policy violation. Not all policies will be able
  to support this (see below).

Proposed, but not yet implemented actions. Definitions will be added in the
future.

- `block`: Allstar can set a [GitHub Status
  Check](https://docs.github.com/en/github/collaborating-with-pull-requests/collaborating-on-repositories-with-code-quality-features/about-status-checks)
  and block any PR in the repository from being merged if the check fails.
- `email`: Allstar would send an email to the repository administrator(s).
- `rpc`: Allstar would send an rpc to some organization-specific system.

### **Action configuration**

Two settings are available to configure the issue action:

- `issueLabel` is available at the organization and repository level. Setting it
  will override the default `allstar` label used by Allstar to identify its
  issues.

- `issueRepo` is available at the organization level. Setting it will force all
  issues created in the organization to be created in the repository specified.

## **Policies**

Similar to the Allstar app enable configuration, all policies are enabled and
configured with a yaml file in either the organization's `.allstar` repository,
or the repository's `.allstar` directory. As with the app, policies are opt-in
by default, also the default `log` action won't produce visible results. A
simple way to enable all policies is to create a yaml file for each policy with
the contents:

```
optConfig:
  optOutStrategy: true
action: issue
```

The `fix` action is not implemented in any policy yet, but will be implemented
in those policies where it is applicable soon.

### Branch Protection

This policy's config file is named `branch_protection.yaml`, and the [config
definitions are
here](https://pkg.go.dev/github.com/ossf/allstar@v0.0.0-20210728182754-005854d69ba7/pkg/policies/branch#OrgConfig).

The branch protection policy checks that GitHub's [branch protection
settings](https://docs.github.com/en/github/administering-a-repository/defining-the-mergeability-of-pull-requests/about-protected-branches)
are setup correctly according to the specified configuration. The issue text
will describe which setting is incorrect. See [GitHub's
documentation](https://docs.github.com/en/github/administering-a-repository/defining-the-mergeability-of-pull-requests/about-protected-branches)
for correcting settings.

### Binary Artifacts

This policy's config file is named `binary_artifacts.yaml`, and the [config
definitions are
here](https://pkg.go.dev/github.com/ossf/allstar@v0.0.0-20210728182754-005854d69ba7/pkg/policies/binary#OrgConfig).

This policy incorporates the [check from
scorecard](https://github.com/ossf/scorecard/#scorecard-checks). Remove the
binary artifact from the repository to achieve compliance. As the scorecard
results can be verbose, you may need to run [scorecard
itself](https://github.com/ossf/scorecard) to see all the detailed information.

### Outside Collaborators

This policy's config file is named `outside.yaml`, and the [config definitions
are
here](https://pkg.go.dev/github.com/ossf/allstar@v0.0.0-20210728182754-005854d69ba7/pkg/policies/outside#OrgConfig).

This policy checks if any [Outside
Collaborators](https://docs.github.com/en/organizations/managing-access-to-your-organizations-repositories/adding-outside-collaborators-to-repositories-in-your-organization)
have either administrator(default) or push(optional) access to the
repository. Only organization members should have this access, as otherwise
untrusted members can change admin level settings and commit malicious code.

### SECURITY.md

This policy's config file is named `security.yaml`, and the [config definitions
are
here](https://pkg.go.dev/github.com/ossf/allstar@v0.0.0-20210728182754-005854d69ba7/pkg/policies/security#OrgConfig).

This policy checks that the repository has a security policy file in
`SECURITY.md` and that it is not empty. The created issue will have a link to
the [GitHub
tab](https://docs.github.com/en/code-security/getting-started/adding-a-security-policy-to-your-repository)
that helps you commit a security policy to your repository.

### Future Policies

- Ensure dependabot is enabled.
- Check that dependencies are pinned/frozen.
- More [checks from scorecard](https://github.com/ossf/scorecard/#scorecard-checks).

## **Example Config Repository**

See [this repo](https://github.com/GoogleContainerTools/.allstar) as an example
of Allstar config being used. As the organization administrator, consider a
README.md with some information on how Allstar is being used in your
organization.

## **Contribute Policies**

[Interface definition.](pkg/policydef/policydef.go)

Both the [SECURITY.md](pkg/policies/security/security.go) and [Outside
Collaborators](pkg/policies/outside/outside.go) policies are quite simple to
understand and good examples to copy.
