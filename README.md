# JSON_Chemical_Formulae_Glossary
A JSON file that contains the name of the chemical compound in "symbolic notation", the English name of the compound, and then its CAS number. A helpful file for useful information.

Located on <a href = "https://en.wikipedia.org/wiki/Glossary_of_chemical_formulae">this page</a> is a list of "common chemical compounds with chemical formulae and CAS numbers, indexed by formula"[[1]](#1). Performing a cursory search online for the same information available in a JSON format seemed to be hopeless. This information must absolutely be readily available in JSON format.

To avoid reinventing the wheel, a search was conducted to find existing tools that would convert Wikipedia table content to more a data science-applicable file-type like CSV or JSON. (While ultimately a JSON file was desired, if there existed a tool that would output a CSV, the conversion from the CSV to the JSON file would be trivial.) There was, thankfully, an extremely useful tool that promised to convert a Wikipedia table to a JSON file. That resource is located <a href = "https://www.wikitable2json.com/">here, online</a>, and <a href = "https://github.com/atye/wikitable-api">here, on Github</a>.

However, even after employing <a href = "https://jsonformatter.curiousconcept.com/">JSON Formatter and Validator</a> to prettify the JSON, the output file was undesirable. The "caption" key seemed to be empty, and the "data" key was an array of arrays. Here is an example of the output file:
```json
{
   "caption":"",
   "data":[
      [
         "Ac2O3",
         "actinium(III) oxide",
         "12002-61-8"
      ],
      [
         "AgBF4",
         "Silver tetrafluoroborate",
         "14104-20-2"
      ],
      ...
   ]
}
```
The desired final file structure must have consistent key-value pairs and completely eliminate any redundancies. For example:
```json
[
  {
     "chemical":"Ac2O3",
     "symbol":"actinium(III) oxide",
     "CASnumber""12002-61-8"
  },
  {
     "chemical":"AgBF4",
     "symbol":"Silver tetrafluoroborate",
     "CASnumber""14104-20-2"
  },
  ...
]
```
A simple iteration procedure was performed to extract the necessary strings out of the undesirable file and reconstructed into the desired format. Even though there certainly exist more efficient ways to perform this task, a double-nested `forEach` loop pushing to an empty array in the Google Chrome Dev Tools console created the desired output JSON file.
```js
let undesirableFileStructure = returnedAPICall
, newStuff = [];
undesirableFileStructure.forEach(itemInOriginalFile => {
  itemInOriginalFile.data.forEach(arrayOfAlphabetizedChemicals => {
    newStuff.push(
      {
        "chemical":arrayOfAlphabetizedChemicals[0],
        "symbol":arrayOfAlphabetizedChemicals[1],
        "name":arrayOfAlphabetizedChemicals[2]
      }
    )
  })
});
copy(JSON.stringify(newStuff));
```
The resulting JSON file is now available.

## References
<li>
<a id = "1">[1]</a>
<a href = "https://en.wikipedia.org/wiki/Glossary_of_chemical_formulae">'Glossary of chemical formulae'</a>, <i>Wikipedia</i>, Wikimedia Foundation, 22 May 2021, https://en.wikipedia.org/wiki/Glossary_of_chemical_formulae. Retrieved June 15, 2021. 
</li>

## Parenthetical
For how to get a global variable in Chrome Dev Tools into clipboard: https://stackoverflow.com/a/30027011
