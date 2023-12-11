# add target

* add target 執行 configure (shell command)

	add\_custom\_command(
**OUTPUT** ${RTSP\_DEP}
**COMMAND** cmd1 
	**COMMAND** cmd2 ...
**WORKING\_DIRECTORY** ${CMAKE\_CURRENT\_BINARY\_DIR}
**COMMENT** "Build rtsp deps"
)
	add\_custom\_target(target **DEPENDS** dep)
