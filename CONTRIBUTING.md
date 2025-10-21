# Contributing to NewHome

Modified contributing from [Frontier](https://github.com/new-frontiers-14/frontier-station-14/blob/master/CONTRIBUTING.md)

If you're considering contributing to NewHome, [Wizard's Den's PR guidelines](https://docs.spacestation14.com/en/general-development/codebase-info/pull-request-guidelines.html) are a good starting point for code quality and version tracking etiquette. Note that we do not have the same master/stable branch distinction.

Importantly, do not make webedits. From the text above:
> Do not use GitHub's web editor to create PRs. PRs submitted through the web editor may be closed without review.

"Upstream" refers to the [/AuroraSong14/Aurora-Song-14](https://github.com/AuroraSong14/Aurora-Song-14) repository that this fork was created from.

# NewHome specific content

In general, anything you create from scratch (vs. modifying something that exists from upstream) should go in an NewHome subfolder, `_NewHome`.

Examples:
- `Content.Server/_NewHome/`
- `Resources/Prototypes/_NewHome/`
- `Resources/Audio/_NewHome/`
- `Resources/Textures/_NewHome/`
- `Resources/Locale/en-US/_NewHome/`
- `Resources/ServerInfo/_NewHome/Guidebook/`

# Changes to upstream files

If you make a change to an upstream C# or YAML file, **you must add comments on or around the changed lines**.
The comments should clarify what changed, to make conflict resolution simpler when a file is changed upstream.
If you make changes to values, to be consistent, leave a comment in the form `NewHome: OLD<NEW`.

For YAML specifically, if you add a component or add a list of contiguous fields, use block comments, but if you make limited edits to a component's fields, comment the fields individually.

For C# files, if you are adding a lot of code, consider using a partial class when it makes sense.

If cherry-picking upstream features, it is best to comment with the PR number that was cherry-picked.

As an aside, fluent (.ftl) files **do not support comments on the same line** as a locale value - leave a comment on the line above if modifying values.

## Examples of comments in upstream or ported files

A single line comment on a changed yml field:
```yml
- type: entity
  id: TorsoHarpy
  name: "harpy torso"
  parent: [PartHarpy, BaseTorso] # NewHome: add BaseTorso
```

A change to a value (note: `OLD>NEW`)
```yml
  - type: Gun
    fireRate: 4 # NewHome: 3>4
    availableModes:
    - SemiAuto
```

A cyborg module with an added moduleId field (inline blank comment), a commented out bucket (inline blank comment), and a DroppableBorgModule that we've added (begin/end block comment).
```yml
  - type: ItemBorgModule
    moduleId: Gardening # NewHome
    items:
    - HydroponicsToolMiniHoe
    - HydroponicsToolSpade
    - HydroponicsToolClippers
    # - Bucket # NewHome
  # NewHome: droppable borg items
  - type: DroppableBorgModule
    moduleId: Gardening
    items:
    - id: Bucket
      whitelist:
        tags:
        - Bucket
  # End NewHome
```

A comment on a new imported namespace:
```cs
using Content.Client._NF.Emp.Overlays; // NewHome
```

A pair of comments enclosing a block of added code:
```cs
component.Capacity = state.Capacity;

component.UIUpdateNeeded = true;

// NewHome: ensure signature colour is consistent
if (TryComp<StampComponent>(uid, out var stamp))
{
    stamp.StampedColor = state.Color;
}
// End NewHome
```

An edit to a Delta-V locale file, note the `OLD>NEW` format and the separate line for the comment.
```fluent
# NewHome: "Job Whitelists">"Role Whitelists"
player-panel-job-whitelists = Role Whitelists
```

# Mapping

For ship submissons, refer to the [Ship Submission Guidelines](https://frontierstation.wiki.gg/wiki/Ship_Submission_Guidelines) on the Frontier wiki.

In general:

NewHome uses specific prototypes for points of interest and ship maps (e.g. to store spawn information, station spawn data, or ship price and categories).  For ships, these are stored in the VesselPrototype (Resources/Prototypes/_NF/Shipyard) or PointOfInterestPrototype (Resources/Prototypes/_NF/PointsOfInterest).  If creating a new ship or POI, refer to existing prototypes, and submit new creations to Resources/Prototypes/_NewHome/Shipyard and Resources/Prototypes/_NewHome/PointsOfInterest respectively.

If you are making changes to a map, check with the map's maintainer (or if none, its author), and avoid having multiple open features with changes to the same map.

Conflicts with maps make PRs mutually exclusive so either your work on the maintainer's work will be lost, communicate to avoid this!

# Before you submit

Double-check your diff on GitHub before submitting: look for unintended commits or changes and remove accidental whitespace or line-ending changes.

Additionally, for PRs that've been open for a long time, if you see `RobustToolbox` in the changed files, you have to revert it. Use `git checkout upstream/master RobustToolbox` (replacing `upstream` with the name of your AuroraSong14/Aurora-Song-14 remote)

# Changelogs

Currently, all changelogs go to the NewHome changelog. The ADMIN: prefix does nothing at the moment.

# Additional resources

If you are new to contributing to SS14 in general, have a look at the [SS14 docs](https://docs.spacestation14.io/) or ask for help in `#dev-discussion` on [Discord](https://discord.gg/zUXmPrwbbM)!

## AI-Generated Content
You may use AI tools to assist with code, but any AI-generated code must be thoroughly tested and audited before submission. Submitting untested or unaudited AI-generated code is not allowed.

AI-generated sprites and art are not allowed to be submitted to the repository.

Trying to PR untested/unaudited AI-generated code or any AI-generated art may result in you being banned from contributing.
