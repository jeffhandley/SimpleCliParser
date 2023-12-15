# UtilityCli

## Quickly build utility applications that parse the command-line

UtilityCli is built for simple utility console application scenarios. As a wrapper around System.CommandLine, UtilityCli provides simple APIs for extracting strongly-typed options and arguments from the command line args. UtilityCli does not require the CLI to be defined/modeled up-front. Instead, the model is progressively defined while extracting the option and argument values.

```csharp
using UtilityCli;

// --org dotnet --repo runtime area-System.Security untriaged --dry-run --issue 40075 --pr 40075
string org = cli.GetRequiredString("org", aliases: ["owner"]);
string repo = cli.GetRequiredString("repo");
int[]? issues = cli.GetInt32s("issue");
int[]? prs = cli.GetInt32s("pr");
bool dryRun = cli.HasFlag("dry-run", shortNames: null);
string[] labels = cli.GetRequiredStrings();

foreach (int issue in issues ?? [])
{
    Console.WriteLine($"Add labels to {org}/{repo}#{issue} (issue): {string.Join(", ", labels)}");
}

foreach (int pr in prs ?? [])
{
    Console.WriteLine($"Add labels to {org}/{repo}#{pr} (PR): {string.Join(", ", labels)}");
}

```

## Approachable, easy, opinionated

Working in happy-path scenarios, UtilityCli allows you to simply call `Parse(args)` in your console application and then extract elements into strongly-typed values. UtilityCli applies some opinionated formatting to your command-line to achieve an easy-to-use API.

* Option names are always prefixed with a double dash `--name`
* Option short names must always be single characters, and are recognized with a single `-n`

UtilityCli is not intended for CLI applications that are distributed to other users; it's intended for utility applications you create for yourself or your team. Because UtilityCli does not require the CLI to be modeled up-front, there are trade-offs in terms of the end-user's experience rearranging options and arguments on the command-line. With a fully-modeled System.CommandLine CLI, there is no ambiguity in terms of how commands, options, and arguments are extracted. Using UtilityCli and skipping the step of modeling your CLI can introduce such ambiguities. For utility apps you author for yourself or your team, and even for single-use applications though, this trade-off can save time and frustration.

If your application matures, UtilityCli can mature with you. The APIs allow gradual or full CLI modeling to occur before parsing, reaching the same point of zero ambiguity you get when using System.CommandLine directly-after all, UtilityCli is simply a convenience layer that wraps around it. A fully-modeled UtilityCli application can easily be refactored to directly use System.CommandLine as well.
