local mhyesTOGGLE = false
local mhnoTOGGLE = false
local fixgrassTOGGLE = false
local unlockallskinTOGGLE = false
local unlockspellTOGGLE = false
local showjungleTOGGLE = false
local nocdTOGGLE = false
local mhnovtwoTOGGLE = false

local libTarget = gg.getRangesList("liblogic.so")

local strA = 0x0
local strB = 0x0
local strC = 0x0
local strD = 0x0
local strE = 0x0
local strF = 0x0

local h1 = 0x0
local h2 = 0x0
local h3 = 0x0
local h4 = 0x0
local h5 = 0x0
local h6 = 0x0

local hx1 = 0x0
local hx2 = 0x0
local hx3 = 0x0
local hx4 = 0x0
local hx5 = 0x0
local hx6 = 0x0
-- script creator: Nok1a (before under the name Platonic/Mr.Dragon Star)
-- Tutorial on how to use:  https://youtu.be/OylUtyEwGVA
-- Use whatever you like in the script in case needed
-- value = Time.timeScale

local hexConvert = 0x0
local dataType = 0x0
local cdOffsetSmall = 0x0
local cdOffsetBig =  0x0
local cbOffsetSmall = 0x0
local anonSpeedOffsetSmall = 0x0
local anonSpeedOffsetBig = 0x0
local anonGroupSearchOffset = 0x0

local game = gg.getTargetInfo() or {x64 = gg.alert('Is game?', 'x64', '', 'x32') == 1}
local x64 = game.x64
local ggg = {}
for a, b in next, gg do
	ggg[a] = b
end
local cfg = loadfile(gg.EXT_CACHE_DIR .. '/' .. gg.getFile():match('[^/]+$'))
cfg = cfg and cfg().version == version and cfg() or {-77, 47, false, false}
local function ml()
	local a = gg.prompt({
		'Camera view A: <Plus Horizontal>[-82;-50]',
		'Camera view B: <Just Vertical>[0;99]',
		'Memory ranges',
		'On Hack ≥ 50% Loading'
	},
	cfg,
	{'number', 'number', 'checkbox', 'checkbox', 'checkbox', 'checkbox'})
	if not a then
		gg.setVisible(false)
		return
	end
	if a[3] then
		local r = {
			'REGION_JAVA_HEAP',
			'REGION_C_HEAP',
			'REGION_C_ALLOC',
			'REGION_C_DATA',
			'REGION_C_BSS',
			'REGION_PPSSPP',
			'REGION_ANONYMOUS',
			'REGION_JAVA',
			'REGION_STACK',
			'REGION_ASHMEM',
			'REGION_VIDEO',
			'REGION_OTHER',
			'REGION_BAD',
			'REGION_CODE_APP',
			'REGION_CODE_SYS'
		}
		local m = {
			'Jh: Java heap',
			'Ch: C++ heap',
			'Ca: C++ Alloc',
			'Cd: C++ .data',
			'Cb: C++ .bss',
			'PS: PPSSPP',
			'A: Anonymous',
			'J: Java',
			'S: Stack',
			'As: Ashmem',
			'V: Video',
			'O: Other',
			'B: Bad',
			'Xa: Code App',
			'Xs: Code system',
		}
		local s = {}
		for z, x in next, gg.getRangesList() do
			s[x.state] = s[x.state] or x['end'] - x.start
			s[x.state] = s[x.state] + (x['end'] - x.start)
		end
		local u = {}
		for z, x in next, s do
			u.GB = u.GB or {}
			u.GB[z] = x / 1000000000
			u.MB = u.MB or {}
			u.MB[z] = (x / 1000000000) == 0 and x / 1000000 or nil
			u.KB = u.KB or {}
			u.KB[z] = (x / 1000000) == 0 and x / 1000 or nil
		end
		for z, x in next, u do
			for q, w in next, x do
				if w > 0 then
					for o, p in next, m do
						if p:match(q .. ':') then
							m[o] = p .. ' [' .. w .. ' ' .. z .. ']'
						end
					end
				end
			end
		end
		local d = {}
		for z, x in next, r do
			d[z] = gg[x] ~= 0
		end
		local c = gg.multiChoice(m, d, 'MEMORY RANGES:')
		if c then
			for z, x in next, c do
				gg[r[z]] = ggg[r[z]]
				r[z] = nil
			end
			for z, x in next, r do
				gg[r[z]] = 0
			end
			gg.saveVariable(r, gg.EXT_CACHE_DIR .. '/' .. gg.getFile():match('[^/]+$') .. '.ranges')
		end
		return ml()
	end
	a[3] = false
	a.version = version
	gg.saveVariable(a, gg.EXT_CACHE_DIR .. '/' .. gg.getFile():match('[^/]+$'))
	for z, x in next, a do
		cfg[z] = x
	end
	if a[1] ~= '0' then
		a[1] = a[1] .. '000000'
		a[1] = '-1000130432' + a[1]
	end
	if a[2] ~= '0' then
		a[2] = a[2] .. '000000'
		a[2] = '1000130432' + (a[2] * 2)
	end
		if a[6] then--Hide name
			gg.setRanges(gg.REGION_C_ALLOC | gg.REGION_OTHER | gg.REGION_ANONYMOUS)
			gg.clearResults()
			gg.searchNumber(game.x64 and "1,157,234,688" or '4,225', 4)
			local found = gg.getResults(gg.getResultsCount())
			gg.clearResults()
			local orval = {'1082130432'}
			for a, b in next, orval do
				orval[a] = b + 0
			end
			local val = {}
			for _, __ in next, found do 
				local c = gg.getValues({
					{address = __.address - (4 * 11), flags = 4, value = 0},
					{address = __.address - (4 * 12), flags = 4, value = 0}
				})
				if c[1].value == 2 then k() end
				if c[1].value == "1093664768" + 0  then
					if c[2].value == orval[1] then
						c[2].value = -1
					elseif c[2].value == -1 then
						c[2].value = orval[1]
					end
					c[2].freeze = true
					val[#val + 1] = c[2]
				end
			end
			for _, __ in next, val do
				gg.addListItems(val)
				gg.removeListItems(val)
				if __.value == -1 then
					gg.toast("Success Hack.")
				else
					gg.toast("Success revert.")
				end
				break
			end
		end
	if true then
		local val = loadfile(gg.EXT_CACHE_DIR .. '/val.lua')
		val = val and val()[16] and val()
		for z, x in next, gg.getValues(val or {}) do
			val = val and x.value == val[z].value and val or nil
		end
		gg.setRanges(gg.REGION_ANONYMOUS)
		gg.clearResults(gg.setVisible(false))
		gg.clearResults()
		val = not val and gg.searchNumber('h0E000000170000000B00000001000000', gg.TYPE_BYTE)--14;256;23;4,294,967,307Q
		local res = gg.getResults(gg.getResultsCount())
		gg.clearResults() 
		for z, x in next, res do
			res[1].value = 9
			res[1].freeze = true
			gg.addListItems(res)
			gg.removeListItems(res)
			gg.saveVariable(res, gg.EXT_CACHE_DIR .. '/val.lua')
			gg.toast('Xray Actived')
		end
		gg.setRanges(gg.REGION_C_ALLOC | gg.REGION_OTHER)
		gg.clearResults()
		gg.searchNumber(x64 and '19,864,223,744,017' or '4,225', x64 and gg.TYPE_QWORD or 4)--1081006571
		local res = gg.getResults(gg.getResultsCount())
		gg.clearResults()
		local f = {}
		for z, x in next, res do
			f[#f + 1] = {address = x.address + (x64 and 0xD0 or 0xB0), flags = 4, value = x.value}
		end
		f = gg.getValues(f)
		local e = {}
		local p = {{}, {-1082130264, -1082129594, -1082128755}}
		for z, x in next, p[2] do
			p[1][x] = z
		end
		p = p[1]
		for z, x in next, f do
			if p[x.value] then
				e[#e + 1] = {address = x.address + 0x4, flags = 4, value = a[1], freeze = true}
				e[#e + 1] = {address = x.address + 0x14, flags = 4, value = a[2], freeze = true}
				gg.addListItems(e)
				gg.removeListItems(e)
				gg.toast('Success hack')
			end
		end
		if a[4] then
			local s = os.time()
			while #e > 0 and os.time() - s < 181 do
				if gg.isVisible() then
					gg.setVisible(false)
					gg.toast('Acces not permitted.')
				end
				for z, x in next, gg.getValues(e) do
					if x.value == 0 then
						gg.addListItems(e)
						z = os.time()
						while os.time() - z < 6 do
							gg.setVisible(false)
						end
						gg.removeListItems(e)
						gg.toast('Script ended')
						e = {}
						break
					end
				end
			end
		end
	end
	local b = os.time()
	while not gg.isVisible() do
		if os.time() - b > 2 then
			return
		end
	end
	return ml()
end
local b = loadfile(gg.EXT_CACHE_DIR .. '/' .. gg.getFile():match('[^/]+$') .. '.ranges')
b = b and b() or {}
for z, x in next, b do
	gg[x] = 0
end

function checkArchitecture()
    local targetInfo = gg.getTargetInfo()
    local arch = gg.getTargetInfo().x64
    gg.alert("Vip Script By Mgubs")
    gg.alert("Script Expired On September 1,2024")
    if arch then
        gg.alert("You Are Running On 64Bit")
        onA = "-2,999,674,700,646,286,368"
    else
        gg.alert("You Are Running On 32Bit")
        onA = "-2,220,275,583,130,009,600"
    end
end

function isProcess64Bit()
  -- Function -> by CmP: https://gameguardian.net/forum/topic/36604-how-to-get-instruction-set-architecture-on-emulator-virtual-memory-addresses/?do=findComment&comment=135506
  local regions = gg.getRangesList()
  local lastAddress = regions[#regions]["end"]
  return (lastAddress >> 32) ~= 0
end

function noselect()
  gg.toast("You not select anything")
end

function cb_nothingFound()
  if gg.getResultsCount() == 0 then
    gg.alert("Search not found, Use Fuzzy or Xa")
    start()
  end
end


local ISA = isProcess64Bit()
local resultsCount = gg.getResultsCount()
gg.clearResults()

function instructions()
  if ISA == false then -- if true then 32 bit else 64 bit
    hexConvert = 0x100000000
    dataType = 4
    cdOffsetSmall = 0xC
    cdOffsetBig =  0xB58
    cbOffsetSmall = 0x1C
    anonSpeedOffsetSmall = 0xE4
    anonSpeedOffsetBig = 0xEC
    anonGroupSearchOffset = 0x30
  else
    hexConvert = 0x0
    dataType = 32
    cdOffsetSmall = 0x18
    cdOffsetBig = 0x16B0
    cbOffsetSmall = 0x38
    cbOffsetBig = 0x48
    anonSpeedOffsetSmall = 0xF4
    anonSpeedOffsetBig = 0xFC
    anonGroupSearchOffset = 0x44
  end
end
function unityVersion()
  gg.searchNumber(":Expected version: ", gg.TYPE_BYTE, gg.setRanges(16384))
  if gg.getResultsCount() == 0 then
    gg.alert("This game is not made with Unity, fuzzy will be automatically activated!")
    gg.alert("Make sure your game is open, don't hide it on the background!")
    randomized()
    os.exit()
  else
    local count = gg.getResultsCount()
    local results = gg.getResults(count)
    values = {}
    for i = 18, count, 18 do
      local resultAddress = results[i].address
      for j = 1, 7 do
        values[#values + 1] = {address = resultAddress + j, flags = gg.TYPE_BYTE}
      end
      values = gg.getValues(values)
    end
    
    local str = ''
    for i, v in ipairs(values) do
      str = str .. string.char(v.value & 0xFF)
    end
    gg.clearResults()
    if values[1].value <= 0x35 and values[2].value == 0x2E and values[3].value <= 0x35 then -- unity versions 5.5 or below
      gg.alert("Unity version "..str..",This version is to old. Use Speedhack through Cb or Fuzzy")
      anonGroupSearchOffset = 0x3C
      cbOffsetSmall = 0x24
      anonSpeedOffsetSmall = 0xBC
    end
    gg.clearResults()
  end
end

function finalSpeedResult()
  resultsCount = gg.getResultsCount()
  local values = gg.getResults(resultsCount)
  local results = {}
  local speed = {}
  for b = 1, #values do
    local loop = 0x0
    for i = 1, 70 do
      results[#results + 1] = {address = values[b].address + loop, flags = gg.TYPE_FLOAT}
      loop = loop + 4
    end
  end
  results = gg.getValues(results)
  for i, v in ipairs(results) do
    if v.value == 1 then
      speed[#speed +1] = {address = v.address, flags = v.flags, name = "Speed"}
      break
    end
  end
  
  if #speed ~= 0 then
    gg.toast("Time.timeScale found")
    gg.addListItems(speed)
    gg.clearResults()
    gg.alert("𝗠𝗴𝘂𝗯𝘀 𝗦𝗽𝗲𝗲𝗱 𝗛𝗮𝗰𝗸 𝗜𝘀 𝗜𝗻 𝗧𝗵𝗲 𝗦𝗮𝘃𝗲 𝗟𝗶𝘀𝘁 𝗘𝗱𝗶𝘁 𝗧𝗵𝗲 𝗦𝗽𝗲𝗲𝗱 𝗕𝘆 𝗙𝗹𝗼𝗮𝘁🔥")
    os.exit()
  end
end

function startxa()
  gg.clearResults()
  gg.setRanges(gg.REGION_CODE_APP)
  gg.searchNumber("h 6A 6F 79 73 74 69 63 6B 20 31 36 20 62 75 74 74 6F 6E 20 31 39")
  if gg.getResultsCount() == 0 then
    gg.alert("Search not found, try Speedhack Cb or Fuzzy")
    start()
  else
    local a = gg.getResults(1)
    gg.clearResults()
    gg.setRanges(gg.REGION_C_DATA | gg.REGION_OTHER)
    gg.searchNumber(a[1].address, dataType)
    resultsCount = gg.getResultsCount()
    a = gg.getResults(1)
    gg.clearResults()
    gg.setRanges(gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC | gg.REGION_OTHER)
    gg.searchNumber(a[1].address + cdOffsetSmall, dataType)
    if gg.getResultsCount() ~= 1 then 
      gg.clearResults()
      gg.setRanges(gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC | gg.REGION_OTHER)
      gg.searchNumber(a[1].address - cdOffsetBig, dataType)
    end
    finalSpeedResult()
  end
end

function filterCb()
  local b = {}
  gg.clearResults()
  gg.setRanges(gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC | gg.REGION_OTHER)
  gg.searchNumber("h 00 00 80 3F CD CC 4C 3E 00 00 00 41 00 00 C8 42 00 00 B4 43 0A D7 23 3C CD CC 4C 3E 00 00 40 3F 00 00 00")
  if gg.getResultsCount() == 0 then
    gg.searchNumber("-9.81000041962", gg.TYPE_FLOAT)
    cb_nothingFound()
    resultsCount = gg.getResultsCount()
    local t = gg.getResults(resultsCount)
    for i, v in ipairs(t) do
      v.address = v.address + 0xC
    end
    gg.loadResults(t)
    gg.refineNumber("1", gg.TYPE_FLOAT)
    cb_nothingFound()
    resultsCount = gg.getResultsCount()
    b = gg.getResults(resultsCount)
  elseif gg.getResultsCount() ~= 0 then
    gg.refineNumber("h 00 00 80 3F", gg.TYPE_FLOAT)
    cb_nothingFound()
    resultsCount = gg.getResultsCount()
    local a = gg.getResults(resultsCount)
    for i, v in ipairs(a) do
      v.flags = gg.TYPE_FLOAT
    end
    b = gg.getValues(a)
  end
  return b
end
function startcb()
  local fakeSpeedPointers = {}
  local b = filterCb()
  for i, v in ipairs(b) do
    if v.value == 1 then
      fakeSpeedPointers[#fakeSpeedPointers + 1] = {address = v.address - anonGroupSearchOffset, flags = gg.TYPE_FLOAT}
      fakeSpeedPointers[#fakeSpeedPointers + 1] = {address = v.address - 0x48, flags = gg.TYPE_FLOAT}
      fakeSpeedPointers[#fakeSpeedPointers + 1] = {address = v.address - 0x44, flags = gg.TYPE_FLOAT}
    end
  end
  gg.setRanges(gg.REGION_C_BSS | gg.REGION_OTHER | gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC)
  gg.loadResults(fakeSpeedPointers)
  gg.searchPointer(0)
  resultsCount = gg.getResultsCount()
  local offsetStart = gg.getResults(resultsCount)
  for i, v in ipairs(offsetStart) do
    v.address = v.address - cbOffsetSmall
  end
  local valu = gg.getValues(offsetStart)
  for i, v in ipairs(valu) do
    if ISA == false then
      v.address = v.value&0xFFFFFFFF
    else
      v.address = v.value
    end
  end
  gg.loadResults(valu)
  finalSpeedResult()
  gg.alert("Mgubs Speedhack is In The save list edit the speed by float🔥")
end


function randomized()
  gg.clearResults()
  gg.setRanges(gg.REGION_ANONYMOUS | gg.REGION_C_ALLOC | gg.REGION_OTHER)
  gg.searchFuzzy('0', gg.TYPE_FLOAT, gg.SIGN_FUZZY_GREATER)
  for i = 1 , 50 do
    gg.searchFuzzy('0', gg.TYPE_FLOAT, gg.SIGN_FUZZY_GREATER)
    gg.refineNumber('0.01~6', gg.TYPE_FLOAT)
    gg.sleep(50)                                                   
  end
  resultsCount = gg.getResultsCount()
  secondResults = gg.getResults(resultsCount)
  local addTables = {}
  local speed = {}
  for i = 1, #secondResults do
    local loops = 0x0
    for b = 1, 200 do
      addTables[#addTables + 1] = {address = secondResults[i].address + loops, flags = gg.TYPE_FLOAT}
      addTables[#addTables + 1] = {address = secondResults[i].address - loops, flags = gg.TYPE_FLOAT}
      loops = loops + 0x4
    end
  end
  addTables = gg.getValues(addTables)
  for i, v in ipairs (addTables) do
    if v.value == 1 then
      speed[#speed +1] = {address = v.address, flags = v.flags, name = "Speed"}
    end
  end
  if #speed ~= 0 then
    gg.toast("Possible speed values found(not guaranteed)")
    gg.addListItems(speed)
    gg.clearResults()
  else
    gg.toast("Nothing found with Fuzzy, shuttind down script")
  end
  os.exit()
end

local functLoop = true
function start()
  if functLoop == true then
    instructions()
    unityVersion()
    functLoop = false
  end
end

---------

function Drone()
function 
split(szFullString, szSeparator) 
local nFindStartIndex = 1 local nSplitIndex = 1 local nSplitArray 
= {} while true do local nFindLastIndex = string.find 
(szFullString, szSeparator, nFindStartIndex) 
if not nFindLastIndex then nSplitArray[nSplitIndex]
 = string.sub(szFullString, nFindStartIndex, string.len (szFullString))
 break end nSplitArray[nSplitIndex]
 = string.sub (szFullString, nFindStartIndex, nFindLastIndex - 1) nFindStartIndex 
= nFindLastIndex + string.len (szSeparator) nSplitIndex 
= nSplitIndex + 1 end return 
nSplitArray end function 
xgxc(szpy, qmxg) for x = 1, #(qmxg) do xgpy = szpy + qmxg[x]["offset"] xglx 
= qmxg[x]["type"] xgsz = qmxg[x]["value"] xgdj = qmxg[x]["freeze"] if xgdj 
== nil or xgdj == "" then gg.setValues({[1] = {address = xgpy, flags = xglx, value 
= xgsz}}) else gg.addListItems({[1] = {address = xgpy, flags = xglx, freeze
 = xgdj, value = xgsz}}) end xgsl = xgsl + 1 xgjg = true end end function
 xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList() gg.clearResults() gg.setRanges(qmnb[1]["memory"])
 gg.searchNumber(qmnb[3]["value"], qmnb[3]["type"]) if gg.getResultCount() 
== 0 then else gg.refineNumber(qmnb[3]["value"], qmnb[3]["type"]) gg.refineNumber(qmnb[3]["value"], qmnb[3]["type"]) gg.refineNumber(qmnb[3]["value"], qmnb[3]["type"]) if gg.getResultCount() == 0 then else sl = gg.getResults(999999) sz = gg.getResultCount() xgsl = 0 if sz > 999999 then sz = 999999 end for i = 1, sz do pdsz = true for v = 4, #(qmnb) do if pdsz == true then pysz = {} pysz[1] = {} pysz[1].address = sl[i].address + qmnb[v]["offset"] pysz[1].flags = qmnb[v]["type"] szpy = gg.getValues(pysz) pdpd = qmnb[v]["lv"] .. ";" .. szpy[1].value szpd = split(pdpd, ";") tzszpd = szpd[1] pyszpd = szpd[2] if tzszpd == pyszpd then pdjg = true pdsz = true else pdjg = false pdsz = false end end end if pdjg == true then szpy = sl[i].address xgxc(szpy, qmxg) end end if xgjg == true then else end end end end function SearchWrite(Search, Write, Type) gg.clearResults() gg.setVisible(false) gg.searchNumber(Search[1][1], Type) local count = gg.getResultCount() local result = gg.getResults(count) gg.clearResults() local data = {} local base = Search[1][2] if (count > 0) then for i, v in ipairs(result) do v.isUseful = true end for k=2, #Search do local tmp = {} local offset = Search[k][2] - base local num = Search[k][1] for i, v in ipairs(result) do tmp[#tmp+1] = {} tmp[#tmp].address = v.address + offset tmp[#tmp].flags = v.flags end tmp = gg.getValues(tmp) for i, v in ipairs(tmp) do if ( tostring(v.value) ~= tostring(num) ) then result[i].isUseful = false end end end for i, v in ipairs(result) do if (v.isUseful) then data[#data+1] = v.address end end if (#data > 0) then local t = {} local base = Search[1][2] for i=1, #data do for k, w in ipairs(Write) do offset = w[2] - base t[#t+1] = {} t[#t].address = data[i] + offset t[#t].flags = Type t[#t].value = w[1] if (w[3] == true) then local item = {} item[#item+1] = t[#t] item[#item].freeze = true gg.addListItems(item) end end end gg.setValues(t) gg.addListItems(t) else return false end else return false end end

UBED = gg.prompt({
" \nᴅʀᴏɴᴇ ᴠɪᴇᴡ [0;12]", 
"ʙᴀᴄᴋ", 
},{
},{
"number",
"checkbox", 
})

if UBED == nil then else
if UBED[1] == "0" then
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "1" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1084227584",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "2" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1092616192",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "3" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1095712192",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "4" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1097859072",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "5" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1101004800",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "6" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1103626240",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "7" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1106247680",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "8" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1108082688",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "9" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1109393408",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "10" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1110704128",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "11" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1112014848",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "12" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "1112512598",offset = "-128",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "1" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1080452710",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "2" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1078774989",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "3" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1077097267",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "4" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1075419546",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "5" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1073741824",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "6" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1072902963",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "7" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1072064102",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "8" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1071225242",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "10" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1070386381",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "11" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1069547520",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
elseif UBED[1] == "12" then
qmnb = {{memory = "4"},{name = ""},{value = "4294968312.0",type = "64"},{lv = "1081006571",offset = "-168",type = "32"}}
qmxg = {{value = "-1060426515",offset = "-144",type = "4"}}
xqmnb(qmnb)
gg.clearResults()
gg.clearResults()
gg.clearList()
gg.clearList()
end

if UBED[2] == true then return menu end end HOMEDM = -1 end

function mhON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"CanSight",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 1
        h1 = "0x"..v.Offset..""
       end
       end
    local libTarget = gg.getRangesList("liblogic.so")

    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local a = h1
            local address = name.start + a
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx1 = (tostring(currentValue[1].value))
            end
            end
    mhyesTOGGLE = true
    gg.toast("Maphack Enabled")
    local a = h1
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + a, flags = gg.TYPE_QWORD, value = onA}
            })
            break
        end
    end
end


function mhOFF()
    mhyesTOGGLE = false
    gg.toast("Maphack Disabled")
    local a = h1
    local aa = hx1
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + a, flags = gg.TYPE_QWORD, value = aa}
            })
            break
        end
    end
end

function mhnoON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"UpdateEyeLayer",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 2
        h2 = "0x"..v.Offset..""
       end
       end
       for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local b = h2
            local address = name.start + b
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx2 = (tostring(currentValue[1].value))
            end
            end
    mhnoTOGGLE = true
    gg.toast("Maphack No Icon Enabled")
    local b = h2
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + b, flags = gg.TYPE_QWORD, value = onA}
            })
            break
        end
    end
end

function mhnoOFF()
    mhnoTOGGLE = false
    gg.toast("Maphack No Icon Disabled")
    local b = h2
    local bb = hx2
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + b, flags = gg.TYPE_QWORD, value = bb}
            })
            break
        end
    end
end

function mhnovtwoON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"CanSight",
"UpdateEyeLayer",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 2
        h1 = "0x"..v.Offset..""
       end
       end
for k ,v in ipairs(search[2]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 2
        h2 = "0x"..v.Offset..""
       end
       end
       for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local a = h1
            local address = name.start + a
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx1 = (tostring(currentValue[1].value))
            end
            end
            for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local b = h2
            local address = name.start + b
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx2 = (tostring(currentValue[1].value))
            end
            end
    mhnovtwoTOGGLE = true
    local a = h1
    local b = h2
    local bba = hx1
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + a, flags = gg.TYPE_QWORD, value = onA}
            })
            gg.sleep(1000)
            gg.setValues({
                {address = name.start + b, flags = gg.TYPE_QWORD, value = onA}
            })
            gg.setValues({
                {address = name.start + a, flags = gg.TYPE_QWORD, value = bba}
            })
            break
        end
    end
end

function mhnovtwoOFF()
    mhnovtwoTOGGLE = false
    local b = h2
    local bba = hx2
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + b, flags = gg.TYPE_QWORD, value = bba}
            })
            break
        end
    end
end

function fixgrassON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"SetGrassLayer",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 2
        h3 = "0x"..v.Offset..""
       end
       end
for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local c = h3
            local address = name.start + c
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx3 = (tostring(currentValue[1].value))
            end
            end
    fixgrassTOGGLE = true
    gg.toast("Fix Grass Enabled")
    local c = h3
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + c, flags = gg.TYPE_QWORD, value = onA}
            })
            break
        end
    end
end

function fixgrassOFF()
    fixgrassTOGGLE = false
    gg.toast("Fix Grass Disabled")
    local c = h3
    local cc = hx3
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + c, flags = gg.TYPE_QWORD, value = cc}
            })
            break
        end
    end
end

function unlockallskinON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"BLuckyBoyFreeSkin",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ChooseHeroMgr' then  ---Class Name 2
        h4 = "0x"..v.Offset..""
       end
       end
       for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local d = h4
            local address = name.start + d
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx4 = (tostring(currentValue[1].value))
            end
            end
    unlockallskinTOGGLE = true
    gg.toast("Unlock All Skin Enabled")
    local d = h4
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + d, flags = gg.TYPE_QWORD, value = onA}
            })
            break
        end
    end
end

function unlockallskinOFF()
    unlockallskinTOGGLE = false
    gg.toast("Unlock All Skin Disabled")
    local d = h4
    local dd = hx4
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + d, flags = gg.TYPE_QWORD, value = dd}
            })
            break
        end
    end
end

function unlockspellON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"BCustomRoomFreeSkill",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ChooseHeroMgr' then  ---Class Name 2
        h5 = "0x"..v.Offset..""
       end
       end
       for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local e = h5
            local address = name.start + e
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx5 = (tostring(currentValue[1].value))
            end
            end
    unlockspellTOGGLE = true
    gg.toast("Unlock All Spell Enabled")
    local e = h5
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + e, flags = gg.TYPE_QWORD, value = onA}
            })
            break
        end
    end
end

function unlockspellOFF()
    unlockspellTOGGLE = false
    gg.toast("Unlock All Spell Disabled")
    local e = h5
    local ee = hx5
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + e, flags = gg.TYPE_QWORD, value = ee}
            })
            break
        end
    end
end

function showjungleON()
io.open("LiblogicRequirment.lua","w+"):write(gg.makeRequest("https://pastebin.com/raw/MZe46502").content):close()
require('LiblogicRequirment')
Il2cpp({il2cppVersion = 27})
search = Il2cpp.FindMethods({
"CanSight",
})
for k ,v in ipairs(search[1]) do
    if v.ClassName == 'ShowEntity' then  ---Class Name 2
        h6 = "0x"..v.Offset..""
       end
       end
       for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            local f = h6
            local address = name.start + f
            local currentValue = gg.getValues({{address = address, flags = gg.TYPE_QWORD}})
            
            hx6 = (tostring(currentValue[1].value))
            end
            end
    showjungleTOGGLE = true
    gg.toast("Show Jungle Enabled")
    local f = h6
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + f, flags = gg.TYPE_QWORD, value = onA}
            })
            break
        end
    end
end

function showjungleOFF()
    showjungleTOGGLE = false
    gg.toast("Show Jungle Disabled")
    local f = h6
    local ff = hx6
    local libTarget = gg.getRangesList("liblogic.so")
    for _, name in ipairs(libTarget) do
        if name.state == "Xa" then
            gg.setValues({
                {address = name.start + f, flags = gg.TYPE_QWORD, value = ff}
            })
            break
        end
    end
end

function nocdON()
nocdTOGGLE = true
gg.setRanges(gg.REGION_ANONYMOUS)
gg.searchNumber('0D;2098082D;2100252:9', gg.TYPE_DWORD)
gg.getResults(9)
gg.refineNumber('0', gg.TYPE_DWORD)
gg.getResults(9)
gg.editAll('20,000,009', gg.TYPE_DWORD)
gg.clearResults()
end

function nocdOFF()
nocdTOGGLE = false
gg.setRanges(gg.REGION_ANONYMOUS)
gg.searchNumber('20,000,009D;2098082D;2100252:9', gg.TYPE_DWORD)
gg.getResults(9)
gg.refineNumber('20,000,009', gg.TYPE_DWORD)
gg.getResults(9)
gg.editAll('0', gg.TYPE_DWORD)
gg.clearResults()
end

function nograss()
gg.clearResults()
gg.setRanges(gg.REGION_C_ALLOC)
gg.searchNumber("4,555,712,707,545,792,512;1,060,709,522", gg.TYPE_QWORD)
gg.sleep(500)
gg.refineNumber("1,060,709,522", gg.TYPE_QWORD)
gg.getResults(99)
gg.editAll("99999999999", gg.TYPE_QWORD)
gg.toast("𝗡𝗢 𝗚𝗥𝗔𝗦𝗦 𝗔𝗖𝗧𝗜𝗩𝗔𝗧𝗘𝗗!⚡")
gg.sleep("2000")
gg.clearResults()
end

function spamchat()
gg.clearResults()
   gg.setRanges(gg.REGION_ANONYMOUS)
   gg.searchNumber("-4389320786825969664;3272998912;5000D", gg.TYPE_QWORD)
   t = gg.getResults(100)
   for i, i in ipairs(t) do
    if i.value == "5000" then
     i.value = "70"
     i.freeze = true
    end
   end
   gg.setValues(t)
   t = n
   gg.clearResults()
gg.toast("𝗦𝗣𝗔𝗠 𝗖𝗛𝗔𝗧 𝗔𝗖𝗧𝗜𝗩𝗔𝗧𝗘𝗗!⚡")
end

function esp()
gg.searchNumber("-2147483648;-2147483648;-2147483648;1065353216;1065353216;1065353216;1065353216;0;1082130432;11F;0;0;0::49",gg.TYPE_DWORD,false,gg.SIGN_EQUAL,0,-1,0)gg.refineNumber("0", 4, false, 536870912, 0, -1, 0)
gg.refineAddress("0", -1, 4, 536870912, 0, -1, 0)
gg.getResults(100)

gg.editAll("1112014848",gg.TYPE_DWORD)gg.clearResults()

gg.toast(" 𝗘𝗦𝗣 𝗔𝗖𝗧𝗜𝗩𝗔𝗧𝗘𝗗!⚡")
end

function threeD()
gg.clearResults()
gg.setRanges(gg.REGION_ANONYMOUS)
gg.searchNumber(";Ẹ䃵긔섯휊䃳炤䈫馚䈳")
goblok = gg.getResults(100)
gg.editAll(";踦䅏炤삵⑍붺㌳䆫祠䊲", gg.TYPE_WORD)
gg.clearResults()
gg.toast("3𝗗 𝗠𝗔𝗣")
gg.sleep("2000")
gg.clearResults()
end

function maxfps()
for _FORV_8_, _FORV_9_ in ipairs({56277916, 56278756, 56279172, 56277200, 56279652, 56280572, 56280740}) do
gg.setValues({[1] = {address = gg.getRangesList("liblogic.so")[1].start + _FORV_9_, value = "h200080D2", flags = 4}, [2] = {address = gg.getRangesList("liblogic.so")[1].start + _FORV_9_ + 4, value = "hC0035FD6", flags = 4}})
gg.clearResults()
gg.toast("𝗠𝗔𝗫 𝗥𝗘𝗙𝗥𝗘𝗦𝗛 𝗥𝗔𝗧𝗘 𝗔𝗖𝗧𝗜𝗩𝗔𝗧𝗘𝗗!⚡")
end
end

function hidename()
gg.setRanges(gg.REGION_C_ALLOC)
gg.clearResults()
gg.searchNumber("4697254412429557760", gg.TYPE_QWORD)
gg.getResults(9999)
gg.editAll("-9999999", gg.TYPE_QWORD)
gg.clearResults()
gg.toast("𝗛𝗜𝗗𝗘 𝗡𝗔𝗠𝗘 𝗔𝗖𝗧𝗜𝗩𝗔𝗧𝗘𝗗!⚡")
gg.sleep("2000")
gg.clearResults()
end

function junglehero()
gg.setRanges(gg.REGION_ANONYMOUS)
gg.clearResults()
gg.alert("You must select miya first after you execute it,after you change the Code Just Enter the Selected Hero")
-- Define the search string
local searchString = "1;193,273,528,320;42,949,673,005:9"
gg.setVisible(false)
-- Search for the defined string
gg.searchNumber(searchString, gg.TYPE_QWORD)
while true do
    if gg.isVisible(true) then
      gg.setVisible(false)
    end
-- Refine the search to only include the number "1"
gg.refineNumber("1", gg.TYPE_QWORD)

-- Check if any results were found
local results = gg.getResults(gg.getResultsCount())

if #results == 0 then
  gg.toast('No results found. Script terminated.')
  return
end

gg.toast('Results found: ' .. #results)

-- Prompt the user to enter the value to edit all found results
local input = gg.prompt({'Enter Jungle Code'}, {0}, {'number'})

-- Check if the user provided a value
if input == nil then
  gg.toast('No value entered. Script terminated.')
  return
end

local newValue = input[1]

-- Modify the found values
for i, v in ipairs(results) do
  results[i].value = newValue
end

-- Apply the modifications
gg.setValues(results, gg.TYPE_DWORD)

-- Provide feedback to the user
gg.toast('All values updated to ' .. newValue)
gg.clearResults()
end
end

function lololo()
gg.alert("This Is For Resetting Banned Account Only")
gg.alert("Once Executed Restart All")
file = gg.FILES_DIR:gsub(gg.PACKAGE:gsub("%p", function(...)
        return "%" .. (...)
    end) .. ".-$", "", 1) .. "com.mobile.legends/shared_prefs/"
    data = io.open(file .. "com.mobile.legends.v2.playerprefs.xml", "r"):read("*all")
    data = data:gsub("\"JsonDeviceID\"%>(.-)%<", function(hex)
        local Hex = {}
        for i = 1, 5 do
            if i == 1 then
                local str = ""
                for i = 1, 56 / 2 do
                    str = str .. string.format("%02x", math.random(0,255))
                end
                Hex[#Hex + 1] = str
            elseif i == 2 or i == 3 or i == 4 then
                local str = ""
                for i = 1, 2 do
                    str = str .. string.format("%02x", math.random(0,255))
                end
                Hex[#Hex + 1] = str  
            elseif i == 5 then      
                local str = ""
                for i = 1, 6 do
                    str = str .. string.format("%02x", math.random(0,255))
                end
                Hex[#Hex + 1] = str
            end
        end
        print(hex)
        return "\"JsonDeviceID\">" .. table.concat(Hex,"-") .. "<"
    end,1)
    io.open(file .. "com.mobile.legends.v2.playerprefs.xml", "w"):write(data):close()
    gg.toast("Reset Account Success")
    end
    
function mhyes()
    if mhyesTOGGLE then
        return "ᗰᗩᑭᕼᗩᑕK IᑕOᑎ︎ (Lobby/InGame)✅"
    else
        return "ᗰᗩᑭᕼᗩᑕK IᑕOᑎ︎(Lobby/InGame)❌"
    end
end

function mhno()
    if mhnoTOGGLE then
        return "ᗰᗩᑭᕼᗩᑕK ᑎO IᑕOᑎ (Lobby/Draftpick)✅"
    else
        return "ᗰᗩᑭᕼᗩᑕK ᑎO IᑕOᑎ (Lobby/Draftpick)❌"
    end
end

function fixgrass()
    if fixgrassTOGGLE then
        return "ᖴI᙭ᘜᖇᗩՏՏ︎(Lobby/Picking/Draftpick)✅"
    else
        return "ᖴI᙭ᘜᖇᗩՏՏ︎(Lobby/Picking/Draftpick)❌"
    end
end

function unlockallskin()
    if unlockallskinTOGGLE then
        return "ᑌᑎᒪOᑕK ᗩᒪᒪ ՏKIᑎ︎(Lobby/Picking)✅"
    else
        return "ᑌᑎᒪOᑕK ᗩᒪᒪ ՏKIᑎ︎(Lobby/Picking)❌"
    end
end

function unlockspell()
    if unlockspellTOGGLE then
        return "ᑌᑎᒪOᑕK ᗩᒪᒪ Տᑭᗴᒪᒪ︎(Lobby/Picking)✅"
    else
        return "ᑌᑎᒪOᑕK ᗩᒪᒪ Տᑭᗴᒪᒪ︎(Lobby/Picking)❌"
    end
end

function showjungle()
    if showjungleTOGGLE then
        return "ՏᕼOᗯ ᒍᑌᑎᘜᒪᗴ︎(INGAME)✅"
    else
        return "ՏᕼOᗯ ᒍᑌᑎᘜᒪᗴ︎(INGAME)❌"
    end
end

function nocd()
    if nocdTOGGLE then
        return "ᑎO ᑕOOᒪᗪOᗯᑎ︎✅"
    else
        return "ᑎO ᑕOOᒪᗪOᗯᑎ︎❌"
    end
end

function mhnovtwo()
    if mhnovtwoTOGGLE then
        return "ᗰᗩᑭᕼᗩᑕK ᑎO IᑕOᑎ ᐯ2(Ingame)✅"
    else
        return "ᗰᗩᑭᕼᗩᑕK ᑎO IᑕOᑎ ᐯ2(Ingame)❌"
    end
end

function minimize_script()
    gg.toast("Minimizing script")
    while true do
        if gg.isVisible(true) then
            gg.setVisible(false)
            break
        end
        gg.sleep(100)
    end
end

minimize_script()
checkArchitecture()

while true do
    local menu = gg.choice({
        "ᗰIᑎIᗰIᘔᗴ︎",
        "ᗴ᙭IT︎",
        "ᖇᗴՏᗴT︎",
        "\n\n🇷 🇪 🇦 🇱  🇲 🇦 🇹 🇨 🇭  🇭 🇦 🇨 🇰 🇸 \n" ..
        "ᗪᖇOᑎᗴᐯIᗴᗯ︎",
        "ᗪᖇOᑎᗴᐯIᗴᗯ︎ V2",
        mhyes(),
        mhno(),
        mhnovtwo(),
        fixgrass(),
        showjungle(),
        "ᗴՏᑭ",
        "3ᗪ ᗰᗩᑭ",
        "ᕼIᗪᗴ ᑎᗩᗰᗴ",
        "ᑎO ᘜᖇᗩՏՏ",
        "Տᑭᗩᗰ ᑕᕼᗩT(ᖇIՏK☢)",
        "ᗰᗩ᙭ᖴᑭՏ",
        "\n\n🇨 🇺 🇸 🇹 🇴 🇲  🇴 🇳 🇱 🇾  🇭 🇦 🇨 🇰 🇸 \n" ..
        unlockallskin(),
        unlockspell(),
        nocd(),
        "ՏᑭᗴᗴᗪᕼᗩᑕK(Custom Only)",
        "ᒍᑌᑎᘜᒪᗴ ᕼᗴᖇO(Custom Practice Mode Only)",
        "\n\nReset Account For Banned Account Only\n" ..
        "ᖇᗴՏᗴT ᗷᗩᑎᑎᗴᗪ ᗩᑕᑕOᑌᑎT"
    }, nil, "(MGUBS)ᗩᒪᒪ ᐯᗴᖇՏIOᑎ︎ 32/64ᗷIT︎\n")
    
    if menu == 1 then
        minimize_script()
    elseif menu == 2 then
        gg.toast("Exiting script")
        mhOFF()
        mhnoOFF()
        mhnovtwoOFF()
        fixgrassOFF()
        unlockallskinOFF()
        unlockspellOFF()
        showjungleOFF()
        nocdOFF()
        os.exit()
    elseif menu == 3 then
        mhOFF()
        mhnoOFF()
        fixgrassOFF()
        unlockallskinOFF()
        unlockspellOFF()
        showjungleOFF()
        nocdOFF()
    elseif menu == 4 then
        Drone()
    elseif menu == 5 then
        return ml()
    elseif menu == 6 then
        if mhyesTOGGLE then
            mhOFF()
        else
            mhON()
        end
    elseif menu == 7 then
        if mhnoTOGGLE then
            mhnoOFF()
        else
            mhnoON()
        end
    elseif menu == 8 then
        if mhnovtwoTOGGLE then
            mhnovtwoOFF()
        else
            mhnovtwoON()
        end
    elseif menu == 9then
        if fixgrassTOGGLE then
            fixgrassOFF()
        else
            fixgrassON()
        end
    elseif menu == 10 then
        if showjungleTOGGLE then
            showjungleOFF()
        else
            showjungleON()
        end
    elseif menu == 11 then
        esp()
    elseif menu == 12 then
        threeD()
    elseif menu == 13 then
        hidename()
    elseif menu == 14 then
        nograss()
    elseif menu == 15 then
        spamchat()
    elseif menu == 16 then
        maxfps()
    elseif menu == 17 then
        if unlockallskinTOGGLE then
            unlockallskinOFF()
        else
            unlockallskinON()
        end
    elseif menu == 18 then
        if unlockspellTOGGLE then
            unlockspellOFF()
        else
            unlockspellON()
        end
    elseif menu == 19 then
        if nocdTOGGLE then
            nocdOFF()
        else
            nocdON()
        end
    elseif menu == 20 then
        startcb()
    elseif menu == 21 then
        junglehero()
    elseif menu == 22 then
        lololo()
        end
        end
