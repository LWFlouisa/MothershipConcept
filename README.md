# Mothership Concept
Suppose you had a mothership / narrow AGI carrier. It would act as a larger metaprogram that would deliver samller more specific narrow AI applications for different use cases. This is dervied from the naval concept of the "aircraft carrier" that carries airplanes on the open sea.

### Proof Of Concept
~~~ruby
require_relative "_modules/mothership.rb"

def create_directories
  system("mkdir _data; cd _data; mkdir ainput; cd ainput; touch input.txt")
  system("mkdir _functionlist; cd _functionlist; mkdir functions; cd functions; touch activities.txt")
  system("mkdir _imaginedpath; cd _imaginedpath; mkdir brains; mkdir prolog; mkdir outcomes")
  system("mkdir _narratives; cd _narratives, mkdir outcomes; cd outcomes; touch character_fates.txt; touch dating_outcomes.txt")
end

# create_directories

initializer      = File.read("_data/ainput/input.txt").strip.to_i
function         = File.readlines("_functionlist/functions/activities.txt")
current_function = function[initializer].to_s.strip

if initializer > 2 # resets function to first function if beyond the max of the function list.
  initializer = 0

  current_function = function[initializer].to_s.strip
end

#puts current_function

# From here it chooses a function to perform.
if    current_function == "chatbot"
  Mothership::BotCarrier.chatbot
elsif current_function == "outcome evaluator"
  Mothership::BotCarrier.outcome_evaluator
elsif current_function == "self evaluate"
  Mothership::BotCarrier.self_evaluate
else
  open("_imaginedpath/brains/commentary.aiml", "w") { |f|
          aiml = "<?xml version = '1.0' encoding = 'UTF-8'?>
<aiml version = '1.0.1' encoding = 'UTF-8'>
  <category>
    <pattern>What are you currently doing?</pattern>
    <template>You have not given me a specific function.</template>
  </category>
</aiml>"

    f.puts aiml
  }
end

open("_data/ainput/input.txt", "w") { |f|
  initializer = initializer + 1

  f.puts initializer
}
~~~
