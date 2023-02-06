[//]: # (This file was autogenerated using `zio-sbt-website` plugin via `sbt generateReadme` command.)
[//]: # (So please do not edit it manually. Instead, change "docs/index.md" file or sbt setting keys)
[//]: # (e.g. "readmeDocumentation" and "readmeSupport".)

# ZIO OpenAI

Library for using the [OpenAI API](https://beta.openai.com/docs/introduction/overview)

[![Development](https://img.shields.io/badge/Project%20Stage-Development-green.svg)](https://github.com/zio/zio/wiki/Project-Stages) ![CI Badge](https://github.com/zio/zio-openai/workflows/CI/badge.svg) [![Sonatype Releases](https://img.shields.io/nexus/r/https/oss.sonatype.org/dev.zio/zio-openai_2.13.svg?label=Sonatype%20Release)](https://oss.sonatype.org/content/repositories/releases/dev/zio/zio-openai_2.13/) [![Sonatype Snapshots](https://img.shields.io/nexus/s/https/oss.sonatype.org/dev.zio/zio-openai_2.13.svg?label=Sonatype%20Snapshot)](https://oss.sonatype.org/content/repositories/snapshots/dev/zio/zio-openai_2.13/) [![javadoc](https://javadoc.io/badge2/dev.zio/zio-openai-docs_2.13/javadoc.svg)](https://javadoc.io/doc/dev.zio/zio-openai-docs_2.13) [![ZIO OpenAI](https://img.shields.io/github/stars/zio/zio-openai?style=social)](https://github.com/zio/zio-openai)

## Introduction

This library provides Scala data types and ZIO services for using the OpenAI API.
The [examples](https://github.com/zivergetech/zio-openai/tree/main/zio-openai-examples/src/main/scala/zio/openai/examples) directory contains a
few examples
of how to use the different features of the library.

The following example is the translation of [OpenAI's official quickstart example](https://beta.openai.com/docs/quickstart):

```scala
import zio.{Console, ZIO, ZIOAppDefault}
import zio.openai._
import zio.openai.model.CreateCompletionRequest.Prompt
import zio.openai.model.Temperature

object Quickstart extends ZIOAppDefault {

  def generatePrompt(animal: String): Prompt =
    Prompt.String {
      s"""Suggest three names for an animal that is a superhero.
         |
         |Animal: Cat
         |Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline
         |Animal: Dog
         |Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot
         |Animal: ${animal.capitalize}
         |Names:""".stripMargin
    }

  def loop =
    for {
      animal <- Console.readLine("Animal: ")
      result <- Completions.createCompletion(
        model = "text-davinci-003",
        prompt = generatePrompt(animal),
        temperature = Temperature(0.6)
      )
      _ <- Console.printLine("Names: " + result.choices.flatMap(_.text.toOption).mkString(", "))
    } yield ()

  override def run =
    loop.forever.provide(Completions.default)
}
```

The `Completions.default` layer initializes the OpenAI client with the default zio-http client configuration and uses
ZIO's [built-in configuration
system](https://degoes.net/articles/zio-config) to get the OpenAI API key. The default configuration provider looks for
the API Key in the `OPENAI_APIKEY`
environment variable or the `openAI.apiKey` system property.

If your project is using `zio-http` for other purposes as well and you already have a `Client` layer set up, you can use
the `live` variants of the layers (`Completions.live`) to share the same client.

## Installation

Start by adding `zio-openai` as a dependency to your project:

```scala
libraryDependencies += "dev.zio" %% "zio-openai" % "<version>"
```

## Documentation

Learn more on the [ZIO OpenAI homepage](https://zio.dev/zio-flow/)!

## Contributing

For the general guidelines, see ZIO [contributor's guide](https://zio.dev/about/contributing).

## Code of Conduct

See the [Code of Conduct](https://zio.dev/about/code-of-conduct)

## Support

Come chat with us on [![Badge-Discord]][Link-Discord].

[Badge-Discord]: https://img.shields.io/discord/629491597070827530?logo=discord "chat on discord"
[Link-Discord]: https://discord.gg/2ccFBr4 "Discord"

## License

[License](LICENSE)
