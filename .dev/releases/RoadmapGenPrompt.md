/sc:task Generate a detailed roadmap for the {RELEASE} release --compliance strict
   --strategy systematic --seq                                                                           
                                                                                                         
  ## Objective                                                    

  Execute the Roadmap Generator prompt to transform the cleanup-audit release spec into a deterministic,
  execution-ready roadmap.

  ## Instructions

  1. **Read the generator prompt** at:
     `/config/workspace/SuperClaude_Framework/.dev/.releases/backlog/v1.4-roadmap-gen/SUPERCLAUDE-ROADMAP-GENERATOR-PROMPT.md`

     This is your operating procedure. Follow it exactly as written — all sections, rules, output format,
   and compliance tier classification.

  2. **Read the roadmap input** at:
     `{RELEASESPECPATH}`

     This is the spec to transform. Feed the full contents as the generator's input

  4. **Output**: Write the generated roadmap to:
     `{RELEASEROADMAPOUTPUTPATH}`