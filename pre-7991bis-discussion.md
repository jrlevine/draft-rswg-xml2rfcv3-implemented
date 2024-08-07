Allowed &lt;blockquote&gt; as a child of &lt;aside&gt; and &lt;li&gt;.

- I suspect that this was a piecemeal fix to address a specific problem, rather than a comprehensive review of what makes sense to nest in what.  It would be awesome if there were some sort of principled approach that could have been used here; I would be delighted to hear that there was.

Removed "It is an error to have both a "src" attribute and content in the &lt;artwork&gt; element." from &lt;xref target="element.artwork.attribute.src"/&gt;.

- No comments

Added &lt;u&gt; element.

- I am against this addition to the grammar, and objected when it was first announced. It introduces uncertainty about when it is supposed to be used, and the results can make the RFC difficult to understand. Instead, the author can just annotate the Unicode codepoints that need spelling out themselves, and the RPC can ask authors during AUTH48 about any characters they are concerned with.
- This policy around this construct has changed since it was implemented in the big set of post-7991 changes and the reasoning for the current policy should be noted. The current policy is that it must be used when there is a ‘Unicode’ character that is meaningful for the protocol, but does not need to be used otherwise such as with examples, and when it is used the code point is spelled out. This is to ensure that there is no confusion due to rendering limitations or reader confusion, for what are considered ‘important’ characters. For reference, the implementation in the big set of post-7991 changes was that it had to be used for all ‘Unicode’ characters or xml2rfc generated an error, which is what led to the misuse of other constructs to get around this.
- Those air quotes in the last sentence are part of what leads to the uncertainty. It also doesn't address the fact that an author can often do a better job of listing the required Unicode information themselves.
- I think this is a typical authoring aid.
Since the RFC editor has (and other reviewers have) repeatedly asked for characters we had proposed for examples to be substituted by others, I strongly believe this authoring aid should stay available up to the point when the RFC editor is satisfied with the specific choice of the character. (If I could ask for extensions to this aid, I’d ask for a way to get the ugly JSON notation of a non-BMP character, because I still had to compute this by hand when a specific example character changed.)
- Long message from JCK, more lost due to threading

Removed the  "hanging" attribute of &lt;dl&gt; and replaced it with "newline".

- This seems fine.
- This change is mentioned in Section 1.1, but I haven't actually found it in the document. The only other mention of "hanging" is in 4.8, which is about the &lt;list&gt; element, not &lt;dl&gt;.
- More from Jean at &lt;https://mailarchive.ietf.org/arch/msg/rswg/hd5EYCprx3TU8-XN4xg0fok07Ro&gt;

Added the "indent" attribute to &lt;dl&gt;.

- Like &lt;t indent= this is a bad choice, but it's done now.
- The RPC uses the "indent" attribute to improve the formatting of definition lists, e.g., the abbreviation list in RFC 9562 [1]. We check all three outputs to ensure the formatting is good in all of them.
- Semantically, this usage falls into the duality between definition lists and tables.  Tables can do the space allocation needed here automatically, but in our renderings they have overly heavy decoration, which cannot be reduced to be on par with definition lists. So during authoring and editing, we see a lot of shuffling back and forth between tables and definition lists, just based on which one “looks better” — a nice way to lead the arguments about semantic markup ad absurdum.

Added the "align" attribute to &lt;table&gt;. Made the table title centered under the table.

- &lt;table align=?&gt; seems fine.  It's a widely-used feature. The title centering is odd.  That's a rendering choice and not one that we should be specifying.
- The interesting thing here (and probably why title centering was mentioned) is that an author gets to tweak that aspect of the presentation of tables themselves but not of the table titles. That is a valid design, but it is in no way a natural choice.

Allowed the &lt;name&gt; element many more elements inside of it.

- Again, it would be better to identify which elements here.  &lt;ul&gt; probably doesn't make sense, though perhaps &lt;code&gt; does (though as a matter of style, I generally avoid doing that, it's fine if the format permits inadvisable things).

Redefined &lt;references&gt; to allow &lt;references&gt; within it. In the typical case, an outer &lt;references&gt; will be used to hold an inner &lt;references&gt; for normative references and an inner &lt;references&gt; to hold informative references.

- As a practical matter, this is fine.  That's what we do. It's quite possible that there is a better design here.
- The original design was that there was no nesting, and that the container section for all references sections was auto-generated. I believe that was fine and did not merit any changes. Or, to be more accurate: originally there was just one &lt;references&gt; section, and when the distinction between normative and informative was added, xml2rfc just added the title attribute and added a container. That worked well for a long time, and did not really require to change.
- That (bullet above) seems like a pretty good design to me.  I'd prefer if we revert the change then. (If changes were on the table, I might prefer a &lt;references normative="true"&gt; rather than rely on having a &lt;name&gt; as well.)
- The simple-minded normative/informative split is rather suboptimal. I’d prefer not to pour concrete on having just these two categories. (The style discussion is yet to be had, but it is a *style* discussion.)
- That (bullet above) is an interesting assertion.  I thought that this was so well entrenched it wasn't worth debating. I can see value in rendering documents with a single category (i.e., if there is no normative references section, use a title of "References" for what would otherwise be "Informative References").  I don't see value in creating a richer taxonomy.  Or is that the style discussion you are talking about not having?
- Not in reply to any specific comment, but more the general view to revert or fix this. I agree that this is messy. certainly when I first encountered it I was surprised at the construct.  However, it works, it’s easy to understand and it has been used extensively, so I don’t see any reason to change it now. To me, this belongs in the box of things to review for v4 not v3.1 (or v3.1 if you think we are currently working on v3.0.1) and during those discussions a very different approach may be taken.  For example, the obvious one would be a new construct &lt;references-section&gt;, which then removes the recursive issue.
- I don't see collapsing references into a single list as a style discussion alone.  Currently, the maturity and stability of the normative reference list is scrutinized.  Without this categorization, the relative light touch review on the stability of the informative references might results in all references getting the current "normative reference treatment".
- Collapsing is not what I’d propose.
- Understood. I read your feedback as adding more categories of references.  I understood Martin’s feedback to be collapse the number of categories into one.  I was reacting to using only category for references.

Un-deprecated metadata attributes on the &lt;rfc&gt; element (with the intent to restore v2 (&lt;xref target="RFC7749"/&gt;) semantics of &lt;seriesInfo&gt; as well).

- There is a lot in this so it is difficult to be sure of all the changes here.  AFAICT this is well embedded in practice (i.e. the attributes in  are used) and so should be kept.  However I note that there are outstanding issues here that need to be addressed at a later stage.
- Agreed.  One observation:  AFIACT from the standpoint of an author, the only reason for having both a "docname" attribute on &lt;rfc&gt; and a separate &lt;seriesInfo&gt; element in &lt;front&gt;, especially since they are now being checked for consistency, is to make extra work and make authoring even more nit-picky.  I don't fully understand the arguments for one or the other (or why seriesInfo was added to &lt;front&gt; in the first place), but retaining both is bad news.
- I think we all agree that having the docName in an RFC be the draft is a mistake. We could make it the RFC name or since that duplicates the seriesinfo, we could take it out of the publication grammar. There is a &lt;link&gt; element that also has the draft name in a more reasonable way. Other than that I think the attributes are OK.

Added &lt;contact&gt; element.

- I disagree with this change.  Names are just text.  This element only exists because someone decided that Unicode text couldn't be put in regular text. See also kramdown-rfc {{{x}}} and https://github.com/cabo/kramdown-rfc/wiki/Syntax*-syntax-contact-names for some more color on this one.
- Are you proposing that we create a new section, "1.4 Elements and Attributes Implemented after Original v3 But Now Deprecated", and that this go into there instead? There are a few other things in the current draft that the WG might want to put in such a section.
- What would this mean operationally? Once in force, would all drafts that use a contact element need to be changed to using something else instead before being published as an RFC? (Note that in our bubblespeak we use the term “deprecated” as a synonym for “abolished”, not the usual “marked for eventual abolition later”.)
- The WG can choose either path. If "as implemented" means "as it was implemented", then we would re-issue the XML without the deprecated elements and attributes. If "as implemented" means "as will be implemented as soon as this is published", then the current XML would exist, but the schema used for it would have never been published. Wearing no hat: I think it's fine to republish the XML to something with equivalent human semantics once.
- Please specify. I don’t think that exists at this point, so we’d have to invent something, which is a bit off-topic for an “as implemented” document. I don’t think this document should have any impact at all on what can be published in production 3.0, except for documenting it in a more accessible way. Once we have done that, we can go for a 3.1, then with some  abolished and some new vocabulary, with some of the latter replacing the former.
- As Lewis Carroll pointed out, we can use words to mean what we want them to mean. We can define "deprecated" quite specifically, for example as "should no longer be used, but will have the following effect if you insist on using it." I agree that &lt;contact&gt; is of value in Contributors and Acknowledgements sections, and shouldn't be deprecated there. What we want to deprecate is its misuse.

Allowed multiple &lt;email&gt; elements in &lt;address&gt;.

- No responses

Added "markers" attribute to &lt;sourcecode&gt; element.

- No responses

Added "brackets" attribute to &lt;eref&gt; element.

- No responses

Added &lt;toc&gt; element, added grammar to allow authors section at the end.

- Once again, given that every published RFC uses them, we're stuck with them for now. Everything in &lt;toc&gt; is redundant, but it can be useful if you want to do single pass formatting, or just don't want to recreate the logic that collects the section names and decides which ones to include, so it's OK.
- The second change is an ugly hack I would really like to fix. The current grammar allows any section to have an &lt;author&gt; as an element. In published RFCs the only section that has author elements is the author-address section at the end, but there's nothing in the grammar preventing you from putting &lt;author&gt; (which is not &lt;contact&gt;) in the abstract or the BCP14 boilerplate which would be absurd. There's also nothing that reminds you that the authors at the end are supposed to match the authors at the front. I'd prefer to invent an &lt;author-list&gt; element inserted by the preptool that can only appear as the last thing in &lt;back&gt; and put the copy of the authors there.
- I think that neither is good and would prefer they not have happened.  Can these be added to the deprecated set as well?
- I agree (to previous bullet).  The addition of a TOC is only necessary for publication formats and can therefore be added by the code that creates those publication formats directly in those formats.
- Please remember that section references from other documents point deeply into these forms and don’t have a way distinguish between different variants of these forms, so all publication formats need to agree on what deep link anchors are provided and where they point to.  (ToCs then typically make use of these anchors.) Nailing down the anchors seems to be the most useful function of the preptool. (In the long run, these anchors need to be made available as metadata so authoring tools can check whether a deep link can be made and whether it points to the right passage [and document!]. For now, the XML publication form can serve as the provider of that metadata, even if it is a bit expensive to manage (much larger than the bibxml).
- I would prefer to instead nail down the format of anchors.  The ToC has no effect on that. That is, specify - at a minimum - the "section-3.4.5" that has been used consistently.  That might extend to specifying how to reference paragraphs as well ("section-3.4.5-1.2").
- +1 (to previous bullet). The idea that a processor needs to inspect the XML source (or something equivalent) of the target document just to compute an anchor name really is disturbing.
- It sure would be nice to have an authoring tool that would validate the section links were sane.  How many times have you linked to RFC1234 section 3.2, when you should have linked to RFC1243 section 3.2?  I have. Worse, when one's document references *both* 1234 and 1243 legtimately. When referencing sections in an I-D, the numbering is definitely not stable, and it sure would be nice to have links that were more than just section numbers.  But, that's not exactly a publication problem. Can I even dream that RFCXXXXbis, might have similiar section links as RFCXXXX?
- With rfcxml.xslt, you can link to author-assigned anchors, provided you have the XML source for the target document. Yes, that's an extension, and it can be converted to plain RFCXML mechanically.

Added "editorial" value to the "submissionType" attribute of &lt;rfc&gt; and the &lt;stream&gt; element for the new Editorial stream.

- No responses

Added &lt;list&gt; as a subelement of &lt;t&gt;.

- I’m confused.   is defined as a subelement of  in RFC 7991 section 2.53 BUT it is deprecated as per RFC 7991 section 3.4.  The only change to  post-7991 is to disallow it in , which makes sense as  is deprecated.
- Yeah, this seems like it is in error.  (Yes, I frequently added &lt;list&gt; as a child of &lt;t&gt;, but that was before we got &lt;ul&gt; and &lt;ol&gt;.)

New Attributes for Existing Elements

Added "sortRefs", "symRefs", "tocDepth", and "tocInclude" attributes to &lt;rfc&gt; to cover Processing Instructions (PIs) that were in v2 (&lt;xref target="RFC7749"/&gt;) that are still needed in the grammar. Added "prepTime" to indicate the time that the XML went through a preparation step. Added "version" to indicate the version of RFCXML vocabulary used in the document. Added "scripts" to indicate which scripts are needed to render the document. Added "expiresDate" when an Internet-Draft expires.

- No responses

Added "ascii" attributes to &lt;email&gt;, &lt;organization&gt;, &lt;street&gt;, &lt;city&gt;, &lt;region&gt;, &lt;country&gt;, and &lt;code&gt;. Also added "asciiFullname", "asciiInitials", and "asciiSurname" to &lt;author&gt;. This allows an author to specify their information in their native scripts as the primary entry and still allow the ASCII-equivalent values to appear in the processed documents.

- No responses

Added "anchor" attributes to many block elements to allow them to be linked with &lt;relref&gt; and &lt;xref&gt;.

- [JM] The new attributes added to &lt;xref&gt; (section, relative, and sectionFormat) has allowed the deprecation of &lt;relref&gt;. It is not found in any RFCs as the prep tool converts them to &lt;xref&gt;.

Added the "section", "relative", and "sectionFormat" attributes to &lt;xref&gt;.

- This is really two requests: 1. section + sectionFormat 2. relative. Each addresses a real need, but they also fit into the established bucket of things that likely could have been done more carefully.  The document currently does a poor job of explaining which xref attributes are mutually exclusive or how they interact generally.  The code in xml2rfc for this is complex and hard to explain, but that doesn't mean there can't be a straightforward description provided.  My attempts to do so only led me to conclude that the design wasn't clear.

Added the "numbered" and "removeInRFC" attributes to &lt;section&gt;.

- Again, this is two very different requests. numbered is backwards, but it's done now, so it's hard to object. removeInRFC is not something that the RSWG should have an opinion on.

Added the "removeInRFC" attribute to &lt;note&gt;.

- No responses

Added "pn" to &lt;artwork&gt;, &lt;aside&gt;, &lt;blockquote&gt;, &lt;boilerplate&gt;, &lt;dt&gt;, &lt;figure&gt;, &lt;iref&gt;, &lt;li&gt;, &lt;references&gt;, &lt;section&gt;, &lt;sourcecode&gt;, &lt;t&gt;, and &lt;table&gt; to hold automatically generated numbers for items in a section that don't have their own numbering (namely figures and tables).

- Deprecate it.  Along with the other junk that the prep tool adds.
- +1
-Concur
- "pn" was added to the 7991 syntax at the request of the Web community who wanted the ability to reference particular paragraphs and other non-header locations in published RFCs. It was a complete pain in the ass to specify, but the Web community was pretty adamant about it. I'm fine with the idea of ripping it out now that we have seen it in practice, but it was put there at the strong request of a significant community.
- Documenting it would need to include documentation for both the pre-state and the post-state of the inexplicable breaking change discussed in &lt;https://github.com/ietf-tools/xml2rfc/issues/581&gt;. I think this sentiment is based on a perspective that does not include using the XML form for further processing outside of the RPC publishing pipeline, both for providing new rendering forms and for processing beyond rendering. Factoring out pn/derivedXxx processing from each such new tool is a net win.
- While I agree that this specific construct should go, there is still a benefit to numbering sections, figures and tables and we should not just deprecate this without a plan to replace it. For example, instead of the pair of numbered=“‘ and pn=“”, we could instead have number=“” which is set to an actual number not a pseudo anchor [1], can be set as “none” to indicate unnumbered things, and can be left blank to be filled in by xml2rfc at some point in the document lifecycle. [1] The generation of links such as #section-1.2 from an attribute number=“1.2” is for the HTML renderer, and this pseudo anchor does not need to be in the RFCXML.
- What would be the use case? I always thought it's a strength of xml2rfc to enforce consistent paragraph numbering.
- My assertion is that we do not need this to be recorded in the grammar.  Instead we need a clear algorithm for numbering, such as... &lt;https://mailarchive.ietf.org/arch/msg/rswg/oPN5drCRvG-zmW9GiUiaBI2Tqb0&gt;
- Martin, how is "deprecate it" an appropriate action for the "as implemented" document? Once you answer that, please go reply to https://mailarchive.ietf.org/arch/msg/rswg/erwhHXOeDNCuvClyFgElxSqJD40
- I was happy with Ekr's answer: consensus that something exists has little value.  That only establishes that we aren't epistemically-challenged. The value comes from our process is in documenting what we agree on, namely what we think about the as-implemented document.
- More in the thread about removing items that are already implemented

Added "display" to &lt;cref&gt; to indicate to tools whether or not to display the comment.

- I really don't understand this feature. For one, &lt;cref&gt; is a strictly authoring convenience, used to insert notes from the editor to the reader of a draft.  It has no business in any final document.  See earlier notes about the line between RFC and I-D. &lt;!-- --&gt; is a perfectly good substitute for &lt;cref display="false"&gt;. The default for this attribute is "true", which sort of makes sense from a backwards-compatibility perspective, but optional boolean attributes defaulting to true is generally recognized as a bit of an unwise thing.
- This feature appears in RFC 7991 Section 2.16.2.  I do not believe there has been a change since then.
- Yep. As part of moving functionality from processing instructions into the grammar.

Added "keepWithNext" and "keepWithPrevious" to &lt;t&gt; as a hint to tools that do pagination that they should try to keep the paragraph with the next/previous element.

- Why is this just &lt;t&gt; ?  Are there not other constructs that might equally benefit from pagination hinting?
- This feature appears in RFC 7991 Section 1.3.2..  There was extensive discussion in https://datatracker.ietf.org/doc/html/draft-levkowetz-xml2rfc-v3-implementation-notes-13 about adding these attributes to siblings of &lt;t&gt; but no change is indicated.
- This oversight clearly belongs into the list of things we want to fix in step (2). Are we taking notes of these observations?

Added "indent" to  &lt;t&gt; to allow for explicitly indented paragraphs.

- This is a poor choice in terms of managing the divide between content and presentation.  It is also shockingly hard to style in CSS.  Though I don't know what precipitated the change, I suspect that I would not have chosen this solution.
- Lots from Jean Mahoney; see &lt;https://mailarchive.ietf.org/arch/msg/rswg/G4XbAWEw-VGB9ocTyFNy3X1cJJU&gt;
