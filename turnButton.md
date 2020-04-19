TurnButton.md

An attempt to isolate turn management from game to enable drop-in operation
Please let me know any ideas or issues you find with this.

This version drops into your global script.
It defines these globals:

tB -- table that contains the bulk of the turnButton data AND code
tB_start -- just calls tB.start; work around for broken UI interface to TTS
tB_endTurn -- just calls tB.endTurn; work around for broken UI interface to TTS
tB_endMoveUI -- just calls tB.endMoveUI; work around for broken UI interface to TTS
tB_toggleGlobalPanel -- just calls tB.toggleGlobalPanel; work around for broken UI interface to TTS
doNothing -- just does nothing; work around for broken UI interface to TTS

API
tB.curPlayer -- string color of current player; but use tb.set() to change it

tB.order -- table of strings used as a circular array; you may alter as you like, but should have at least one element.

tB.init() -- initialize the turnButton with "start the game"

tB.hotSeat -- table of players for use in hotSeat mode.  Copied to order at start if it exists.  Players need not be seated at start time, but will be required to sit before ending their turn.

tB.start(player) -- called from UI when a seated player clicks the start button
    currently sets order to getSeatedPlayers() that have hands, re-ordered by hand position around table

tB.next() -- returns player next in order array

tB.prev() -- returns player previous in order array

tB.set(newPlayer) -- set curPlayer to newPlayer if newPlayer is seated

tB.endTurn(player) -- target of UI player end turn button

tB.toggleGlobalPanel() -- hide/show global buttons

tB.save() -- returns a table suitable for combining with other save data in global onSave()

tB.restore(tb.save()) -- takes a table made by tb.save and configures the turnButton accordingly

tB.pass() -- force end of turn

----------------------------------
turnButton calls several routines in Global if they exist:

initGame(player) -- when a player clicks one of the start buttons

onTurnStart() -- when a player's turn is beginning
onTurnEnd() -- when a player's turn is ended
