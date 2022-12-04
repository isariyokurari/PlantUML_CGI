# PlantUML CGI

English | [Japanese](/READMEj.md)

PlantUML_CGI is CGI script to translate from PlantUML encode to png image.
This script is very simple because it works by just Apache and Python.

## Requirement

- plantuml.jar can used.
- Apache server is working, cgi is enabled.
- cgi can exec Python code.

## How to install

Place plantuml to the path for cgi.
- e.g. `/usr/lib/cgi-bin/plantuml`

## How to use

Exec above plantuml CGI with PlantUML encode supplied by query parmeter 'puencode'. CGI return a png image rendered by `plantuml.jar`. For example, get a png image by accessing to URL as like as follow via web browser.
- e.g. `http://<cgi server>/<path to cgi placed>/plantuml?puencode=<PlantUML encode>`

## Use PlantUML_CGI from Redmine

https://github.com/gelin/plantuml-redmine-macro is good plugin because very simple.
Based it and modify `init.rb` then restart Redmine, you can use PlantUML_CGI from Redmine.

1. Apply plantuml-redmine-macro.
2. Modify `init.rb` in plantuml-redmine-macro from
   ```ruby
   image_tag(URI.join(url, encoded).to_s)
   ```
   to
   ```ruby
   image_tag(URI.join(Setting.plugin_plantuml_macro['plantuml_url'], '?puencode=' + encoded).to_s)
   ```
   - The revision of `init.rb` was `658f599972125d928f9b718ec9e22c554590641b`.
3. Restart Redmine
4. Login to Redmine as administrator, open Plugins from Administration menu, enter configure page for plantuml-redmine-macro and set the URL for PlantUML_CGI.
   - e.g. `http://XXX.XXX.XXX.XXX/cgi-bin/plantuml`

## Motivation

I wanted to use PlantUML server on the server running Redmine.
However, I did't want to install apache maven, jetty, Tomcat and Docker.
So, I decided to install PlantUML to the server running Redmine and create CGI script for PlantUML server.

## The version of Python

CGI script was implemented for Python2 and Python3 because I want to use it on Raspbian buster and Ubuntu 22.04 LTS keep default.

## Confirmed environment

| Hardware    | Raspberry-Pi 3B                | GPD-WIN          |
| ---         | ---                            | ---              |
| PRETTY_NAME | Raspbian GNU/Linux 10 (buster) | Ubuntu 22.04 LTS |
| Python      | 2.7.16                         | 3.10.4           |
| PlantUML    | 1.2022.13                      | 1.2022.13        |
| PostgreSQL  | 11.14                          | 14.4             |
| Redmine     | 4.2.3                          | 5.0.2            |
| Ruby        | 2.7.4                          | 3.1.2            |
| Apache      | 2.4.38                         | 2.4.52           |
| gem         | 3.1.6                          | 3.3.7            |
| passenger   | 6.0.15                         | 6.0.14           |
| Graphviz    | 2.40.1                         | 2.43.0           |
| Java        | 11.0.16                        | 11.0.17          |

## Design

Communications between applications

![Sequence Diagram](http://www.plantuml.com/plantuml/png/TP513W8X34NtFGKNq0EuS6VYGXCNUW0oL8DXW3ZLyujDR22PlmF-U_r9ELxF9xVPkvfyblUStCvTViTRUpxagGGcFqdyU65ZoE1H3AnydmhFzHuJjxHKpZRB0aJh9BjtFOk4MFw0ZHjI6jbEtZxz2xaQqa2krDRyeC30HRNOKnJk89M5pK9Bqn-41VG5 "PlantUML_CGI")

Encode / Decode

![Encode/Decode](http://www.plantuml.com/plantuml/png/VL0z3u8m4DtxAnec66GY30uEHZTDN9n9eHT3KjgcFHP_lGUAoALnQdhtFkuzhmBsNU-LHPdT33ttwqLsJaCcqhkdwLksEwe8TIN1_kCjM_48RlGoEt_-p5Nk3jnCxkUtxDpW0yIO5u8ZYCIk858x3ygshjupud7GncnbHWmb1cMZKGWv2JJe6Z_Xni4K0gp-nZW1Z_6ZpUsuyY8voPDByZuMTPDBp-RfFbYlIuaQrXgd82y0 "PlantUML Encode/Decode")

## References

- PlantUML Server Official Page
  https://plantuml.com/en/server
- PlantUML Server source code
  https://github.com/plantuml/plantuml-server
- PlantUML text encode
  https://plantuml.com/en/text-encoding
- PlantUML Server
  http://www.plantuml.com/plantuml
- plantuml-redmine-macro
  https://github.com/gelin/plantuml-redmine-macro

## License

[LICENSE](/LICENSE)
