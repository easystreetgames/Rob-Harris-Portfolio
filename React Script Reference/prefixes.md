# Prefixes

## property value prefixes

random notation (prefixed with ~)
global variable references (prefixed with $)
embedded global variable references (prefixed with <<)
object (local) variable references (prefixed with %)
CSV variable references (prefixed with ^)
JSON variable references (prefixed with &)
data.txt path to save folder (prefixed with *)

## reserved global variable keys

canvas.bg  // Set canvas background color
canvas.scene  // Queue scene transition
canvas.report  // Generate debug report for specific object
canvas.freeze  // freeze canvas render on/off
canvas.tracker  // Generate usage report / Clear usage data

variable event (prefixed with !)
variable prototype (prefixed with +)

### depricate?

canvas.save  // Trigger game save operation
canvas.load  // Trigger game load operation

## object prefixes

_() empty object
@() add prototype to creator

?id() existing object
$id() prototype ($id*amount for multiple copies)
@$id() prototype using creator's transform
