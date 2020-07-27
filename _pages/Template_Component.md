<includeonly>{| class="infobox" cellspacing="5" style="max-width: 30em; font-size: 80%; text-align: left; float: right; border: 1px solid #a0a0a0;"
! colspan="2" style="text-align: center; font-size: 130%;" | <div style="float: left">{{{logo|}}}</div>{{{name|{{PAGENAME}}}}}<div style="clear: left;"></div>
|-
{{#if:{{{project|}}}|
! Project
{{!}} {{{project|}}}
{{!}}-
}} 
{{#if:{{{url|}}}|
! URL
{{!}} {{{url}}}
{{!}}-
}}
{{#if:{{{source|}}}|
! Source
{{!}} {{{source}}}
{{!}}-
}}
{{#if:{{{license|}}}|
! License
{{!}} {{{license}}}
{{!}}-
}}
{{#if:{{{release|}}}|
! Release
{{!}} {{{release}}}
{{!}}-
}}
{{#if:{{{date|}}}|
! Date
{{!}} {{{date}}}
{{!}}-
}}
{{#if:{{{devStatus|}}}|
! [[Development status]]
{{!}} {{{devStatus}}}
{{!}}-
}}
{{#if:{{{supportStatus|}}}|
! [[Support status]]
{{!}} {{{supportStatus}}}
{{!}}-
}}
! colspan=2 style="background: lightgray; font-variant: small-caps; text-align: center" | [[Team]]
|-
! [[Team|Founders]]
| {{{founders|-}}}
|-
! [[Team|Leads]]
| {{{leads|-}}}
|-
! [[Team|Developers]]
| {{{developers|-}}}
|-
! [[Team|Debuggers]]
| {{{debuggers|-}}}
|-
! [[Team|Reviewers]]
| {{{reviewers|-}}}
|-
! [[Team|Support]]
| {{{support|-}}}
|-
! [[Team|Maintainers]]
| {{{maintainers|-}}}
|-
{{#if:{{{otherDevs|}}}|
! [[Team|Other developers]]
{{!}} {{{otherDevs}}}
{{!}}-
}}
{{#if:{{{contributors|}}}|
! [[Team|Contributors]]
{{!}} {{{contributors}}}
{{!}}-
}}
|}{{#if:{{{project|}}}|{{Project|{{{project}}}}}|}}</includeonly>
<noinclude>This template is a sidebar for displaying key statistics about a software [[component]].
==Usage==
<pre>
{{Component
| project = 
| name = 
| url = 
| source = 
| license = 
| release = 
| date = 
| devStatus = 
| supportStatus = 
| founders = 
| leads = 
| developers = 
| debuggers = 
| reviewers = 
| support = 
| maintainers = 
| contributors = 
| otherDevs = 
}}
</pre>

==Example==
{{Component
| project = ImageJ
| name = ImageJ 1.x
| url = https://imagej.net/index.html
| source = {{GitHub | org=imagej | repo=ImageJA | tag=v1.50d}}
| license = [[Public Domain]]
| release = {{Maven | g=net.imagej | a=ij | v=1.50d | label=1.50d}}
| date = Tue Oct 27 14:25:43 CDT 2015
| devStatus = {{DevStatus | developer=yes | incubating=no | obsolete=no}}
| supportStatus = {{SupportStatus | debugger=yes | reviewer=yes | support=yes}}
| founders = {{Person|Rasband}}
| leads = {{Person|Rasband}}
| developers = {{Person|Rasband}}
| debuggers = {{Person|Rasband}}
| reviewers = {{Person|Rasband}}
| support = {{Person|Rasband}}
| maintainers = {{Person|Rasband}}
| contributors = [http://loci.wisc.edu/people/barry-dezonia Barry DeZonia]
| otherDevs = Wayne Rasband (architect)
}}<pre style="overflow:auto">
{{Component
| project = ImageJ
| name = ImageJ 1.x
| url = https://imagej.net/index.html
| source = {{GitHub | org=imagej | repo=ImageJA | tag=v1.50d}}
| license = [[Public Domain]]
| release = {{Maven | g=net.imagej | a=ij | v=1.50d | label=1.50d}}
| date = Tue Oct 27 14:25:43 CDT 2015
| devStatus = {{DevStatus | developer=yes | incubating=no | obsolete=no}}
| supportStatus = {{SupportStatus | debugger=yes | reviewer=yes | support=yes}}
| founders = {{Person|Rasband}}
| leads = {{Person|Rasband}}
| developers = {{Person|Rasband}}
| debuggers = {{Person|Rasband}}
| reviewers = {{Person|Rasband}}
| support = {{Person|Rasband}}
| maintainers = {{Person|Rasband}}
| contributors = [http://loci.wisc.edu/people/barry-dezonia Barry DeZonia]
| otherDevs = Wayne Rasband (architect)
}}
</pre>

{{Clear}}
[[Category:Boxes]]
</noinclude>