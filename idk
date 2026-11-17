-- this was left open-src on purpose, yall can use my code if u want but please give me the credit

local tab = menu.new_tab("kernel's lua", "rbxassetid://134031444146997")
local nav = tab:new_section("modules", "Right")
local page_dd = nav:new_combobox("page", { "main", "visuals", "rage", "chat" }, "main")

local user = "player"
pcall(function() user = game:GetService("Players").LocalPlayer.Name end)

local rgb = Color3.fromRGB
local states = { "shared", "standing", "walking", "slow-walking", "crouching", "in air" }

local head = {
  { k = "aa_on", n = "enabled", t = "b", d = false },
  { k = "aa_state", n = "state", t = "c", o = states, d = "shared" },
}

local vlist = {
  { k = "pred", n = "enabled", t = "b", d = false },
  { k = "ms", n = "predict time", t = "s", lo = 1, hi = 400, d = 200, u = "ms" },
  { k = "lat", n = "add latency", t = "b", d = false },
  { k = "apiv", n = "api velocity", t = "b", d = true },
  { k = "curve", n = "air curve", t = "s", lo = 0, hi = 100, d = 60, u = "%" },
  { k = "smooth", n = "box smoothing", t = "s", lo = 0, hi = 30, d = 14, u = "" },
  { k = "vcap", n = "draw rate cap", t = "s", lo = 30, hi = 240, d = 90, u = "hz" },
  { k = "r_on", n = "enabled", t = "b", d = false },
  { k = "r_mode", n = "mode", t = "c", o = { "mode 1", "mode 2" }, d = "mode 1" },
  { k = "r_col", n = "accent color", t = "p", d = rgb(232, 160, 168) },
  { k = "r_size", n = "size", t = "s", lo = 50, hi = 200, d = 100, u = "%" },
  { k = "r_drift", n = "drift speed (mode 1)", t = "s", lo = 0, hi = 40, d = 6, u = "px/s" },
  { k = "r_any", n = "any enemy death = kill", t = "b", d = false },
}

local alist = {
  { k = "sh", n = "shared", t = "b", d = true, g = "s" },
  { k = "pitch", n = "pitch", t = "c", o = { "Down", "Up", "Zero", "Disabled" }, d = "Down", g = "r" },
  { k = "yaw", n = "yaw offset", t = "s", lo = -180, hi = 180, d = 180, u = "°", g = "r" },
  { k = "mod", n = "yaw modifier", t = "c", o = { "Disabled", "Spin", "Random", "Center", "3-Way", "5-Way", "Sway" }, d = "Disabled", g = "r" },
  { k = "amt", n = "modifier amount", t = "s", lo = 0, hi = 180, d = 30, u = "°", g = "r" },
  { k = "body", n = "body yaw", t = "b", d = true, g = "r" },
  { k = "inv", n = "inverter", t = "c", o = { "left", "right", "jitter", "random" }, d = "left", g = "r" },
  { k = "lim_l", n = "desync limit left", t = "s", lo = 0, hi = 84, d = 58, u = "°", g = "r" },
  { k = "lim_r", n = "desync limit right", t = "s", lo = 0, hi = 84, d = 58, u = "°", g = "r" },
  { k = "lim_rnd", n = "randomize desync limit", t = "b", d = true, g = "r" },
  { k = "up_fix", n = "level pitch when aiming up", t = "b", d = true, g = "r" },
  { k = "lag", n = "fake lag", t = "b", d = true, g = "r" },
  { k = "lag_t", n = "fake lag limit", t = "s", lo = 0, hi = 14, d = 6, u = "t", g = "r" },
  { k = "lag_v", n = "fake lag variability", t = "s", lo = 0, hi = 100, d = 0, u = "%", g = "r" },
  { k = "def", n = "enabled", t = "b", d = false, g = "r" },
  { k = "d_trig", n = "defensive trigger", t = "c", o = { "interval", "after shot", "always" }, d = "interval", g = "d" },
  { k = "d_pitch", n = "defensive pitch", t = "c", o = { "Up", "Down", "Zero", "Jitter" }, d = "Up", g = "d" },
  { k = "d_yaw", n = "defensive yaw", t = "c", o = { "flick", "spin", "random", "sway" }, d = "flick", g = "d" },
  { k = "d_amt", n = "defensive yaw amount", t = "s", lo = 0, hi = 180, d = 140, u = "°", g = "d" },
  { k = "d_dur", n = "burst duration", t = "s", lo = 1, hi = 14, d = 4, u = "t", g = "d" },
  { k = "d_int", n = "burst interval", t = "s", lo = 100, hi = 2000, d = 600, u = "ms", g = "d" },
  { k = "d_shift", n = "shift ticks", t = "s", lo = 0, hi = 14, d = 14, u = "t", g = "d" },
  { k = "d_des", n = "flip desync (84°)", t = "b", d = true, g = "d" },
  { k = "d_rand", n = "randomize bursts", t = "b", d = true, g = "d" },
  { k = "d_lag", n = "force fake lag in burst", t = "b", d = true, g = "d" },
  { k = "d_lag_t", n = "burst fake lag", t = "s", lo = 1, hi = 14, d = 14, u = "t", g = "d" },
  { k = "d_unch", n = "unchoke after burst", t = "b", d = true, g = "d" },
}

local rlist = {
  { k = "rs_on", n = "enabled", t = "b", d = false },
  { k = "rs_pre", n = "pre-resolve unseen", t = "b", d = true },
  { k = "rs_dec", n = "decompose jitter", t = "b", d = true },
  { k = "rs_lead", n = "tick lead", t = "s", lo = 0, hi = 8, d = 1, u = "t" },
  { k = "rs_baim", n = "baim after misses", t = "s", lo = 0, hi = 5, d = 3, u = " misses" },
  { k = "rs_safe", n = "safe point on random jitter", t = "b", d = false },
  { k = "rs_ind", n = "indicator", t = "b", d = true },
  { k = "rs_lim", n = "limit targets", t = "b", d = false },
  { k = "rs_max", n = "max targets", t = "s", lo = 1, hi = 8, d = 3, u = "" },
}

local cfg = {}
for _, l in ipairs({ head, vlist, rlist }) do
  for _, r in ipairs(l) do cfg[r.k] = r.d end
end

local aas = {}
for _, n in ipairs(states) do
  aas[n] = {}
  for _, r in ipairs(alist) do aas[n][r.k] = r.d end
end

local ed, loaded, box = {}, nil, nil

local function enc(v)
  if type(v) == "string" then
    return '"' .. v:gsub("%c", ""):gsub("\\", "\\\\"):gsub('"', '\\"') .. '"'
  end
  return tostring(v)
end

local function col_str(c)
  local ok, s = pcall(function()
    return ("[%d,%d,%d]"):format(math.floor(c.R * 255 + 0.5), math.floor(c.G * 255 + 0.5), math.floor(c.B * 255 + 0.5))
  end)
  return ok and s or "[232,160,168]"
end

local function obj(list, src, extra)
  local p = {}
  for _, r in ipairs(list) do
    local v = src[r.k]
    if v ~= nil then p[#p + 1] = '"' .. r.k .. '":' .. (r.t == "p" and col_str(v) or enc(v)) end
  end
  if extra then p[#p + 1] = extra end
  return "{" .. table.concat(p, ",") .. "}"
end

local function check(r, v)
  if r.t == "b" then
    if type(v) == "boolean" then return v end
  elseif r.t == "s" then
    if type(v) == "number" and v == v then return math.clamp(math.floor(v + 0.5), r.lo, r.hi) end
  else
    for _, o in ipairs(r.o) do
      if o == v then return v end
    end
  end
end

local alpha = "fiTstSXPDcR92ge04UnyKJAGqxjCwZbhIk8lpu1Lz6OMa5BQr3d7YvVHWomEFN"
local key = "k3rn3l_x9"
local rev = {}
for i = 1, 62 do rev[alpha:sub(i, i)] = i - 1 end

local function kb(p)
  return key:byte(p % #key + 1) + p
end

local function seal(s)
  if #s % 2 == 1 then s = s .. " " end
  local out = {}
  for i = 1, #s, 2 do
    local n = 0
    for p = i - 1, i do
      n = n * 256 + (s:byte(p + 1) + kb(p)) % 256
    end
    local c3 = n % 62
    n = math.floor(n / 62)
    local c2 = n % 62
    local c1 = math.floor(n / 62)
    out[#out + 1] = alpha:sub(c1 + 1, c1 + 1) .. alpha:sub(c2 + 1, c2 + 1) .. alpha:sub(c3 + 1, c3 + 1)
  end
  return table.concat(out)
end

local function unseal(t)
  t = t:gsub("%s", "")
  if #t == 0 or #t % 3 ~= 0 then return nil end
  local out = {}
  for i = 1, #t, 3 do
    local a, b, c = rev[t:sub(i, i)], rev[t:sub(i + 1, i + 1)], rev[t:sub(i + 2, i + 2)]
    if not (a and b and c) then return nil end
    local n = a * 3844 + b * 62 + c
    if n > 65535 then return nil end
    local b2 = n % 256
    local b1 = math.floor(n / 256)
    local p = math.floor((i - 1) / 3) * 2
    out[#out + 1] = string.char((b1 - kb(p)) % 256, (b2 - kb(p + 1)) % 256)
  end
  return table.concat(out)
end

local function to_json()
  local st = {}
  for _, n in ipairs(states) do st[#st + 1] = '"' .. n .. '":' .. obj(alist, aas[n]) end
  local rage = obj(head, cfg, '"states":{' .. table.concat(st, ",") .. "}")
  return '{"kernel":true,"visuals":' .. obj(vlist, cfg) .. ',"resolver":' .. obj(rlist, cfg) .. ',"rage":' .. rage .. "}"
end

local function pack()
  return seal(to_json())
end

local function from_json(text)
  local function say(m, ok) pcall(notify, "Config", m, 3, ok == true) end
  if type(text) ~= "string" or text:gsub("%s", "") == "" then return say("Config is empty") end
  local ok, data = pcall(json.parse, unseal(text) or text)
  if not ok or type(data) ~= "table" then return say("Invalid json") end
  if data.kernel ~= true then return say("Not a kernel config") end

  local n = 0
  local vis = type(data.visuals) == "table" and data.visuals or {}
  for _, r in ipairs(vlist) do
    local v
    if r.t == "p" then
      local c = vis[r.k]
      if type(c) == "table" and type(c[1]) == "number" and type(c[2]) == "number" and type(c[3]) == "number" then
        v = rgb(math.clamp(c[1], 0, 255), math.clamp(c[2], 0, 255), math.clamp(c[3], 0, 255))
      end
    else
      v = check(r, vis[r.k])
    end
    if v ~= nil then cfg[r.k] = v; n = n + 1 end
  end

  local rg = type(data.rage) == "table" and data.rage or {}
  for _, r in ipairs(head) do
    local v = check(r, rg[r.k])
    if v ~= nil then cfg[r.k] = v; n = n + 1 end
  end
  local rv = type(data.resolver) == "table" and data.resolver or {}
  for _, r in ipairs(rlist) do
    local v = check(r, rv[r.k])
    if v ~= nil then cfg[r.k] = v; n = n + 1 end
  end
  local ss = type(rg.states) == "table" and rg.states or {}
  for _, name in ipairs(states) do
    local src = ss[name]
    if type(src) == "table" then
      for _, r in ipairs(alist) do
        local v = check(r, src[r.k])
        if v ~= nil then aas[name][r.k] = v; n = n + 1 end
      end
    end
  end

  loaded = nil
  pcall(function() box:set(pack(), true) end)
  say(("Imported %d settings"):format(n), true)
end

local chat_url = "http://78.154.103.2:10720"
local chat_ch = "channel-kernel#0000"
local chat_tok = "69491fc7a6cda6ed05ad0d180fa272d64f39af8b357ea3d8"

local cc = { on = true }
pcall(function()
  local saved = database.read("kernel_chat")
  if type(saved) == "table" and type(saved.on) == "boolean" then cc.on = saved.on end
end)

local clive = {}
local chat_send, chat_build

local function clean(s, n)
  s = (s:gsub("%c", " "))
  local ok, o = pcall(utf8.offset, s, n + 1)
  return ok and o and s:sub(1, o - 1) or s
end

local function wrap(s, w)
  local out, line = {}, ""
  for piece in s:gmatch("%S+") do
    local word = piece
    while (utf8.len(word) or #word) > w do
      if line ~= "" then
        out[#out + 1] = line
        line = ""
      end
      local o = utf8.offset(word, w + 1) or (w + 1)
      out[#out + 1] = word:sub(1, o - 1)
      word = word:sub(o)
    end
    if line == "" then
      line = word
    elseif (utf8.len(line) or #line) + 1 + (utf8.len(word) or #word) <= w then
      line = line .. " " .. word
    else
      out[#out + 1] = line
      line = word
    end
  end
  if line ~= "" then out[#out + 1] = line end
  return out
end

local live, secs = {}, {}

local function new_sec(name, side)
  local s = tab:new_section(name, side or "Left")
  secs[#secs + 1] = s
  return s
end

local function bind(k, el)
  live[#live + 1] = { k = k, el = el }
  return el
end

local function add(sec, r, v)
  if r.t == "b" then return sec:new_toggle(r.n, v) end
  if r.t == "c" then return sec:new_combobox(r.n, r.o, v) end
  if r.t == "p" then return sec:new_colorpicker(r.n, v) end
  return sec:new_slider(r.n, r.lo, r.hi, v, 0, r.u)
end

local pages = {}

pages.main = function()
  local s = new_sec("kernel's lua")
  s:new_label("hi, " .. user)
  s:new_label("you are using v1.2")

  local c = new_sec("configs", "Right")
  box = c:new_textbox("config json", "paste a config here", pack())
  c:new_button("export", function()
    local j = pack()
    pcall(function() box:set(j, true) end)
    pcall(setclipboard, j)
    pcall(notify, "Config", "Config exported", 2, false)
  end)
  c:new_button("import", function()
    local ok, text = pcall(function() return box:get() end)
    if ok and type(text) == "string" then from_json(text) end
  end)
end

pages.visuals = function()
  local p = new_sec("prediction", "Right")
  local r = new_sec("supportive rectangle")
  for i, v in ipairs(vlist) do
    bind(v.k, add(i <= 7 and p or r, v, cfg[v.k]))
  end
end

pages.rage = function()
  local a = new_sec("anti-aim")
  local d = new_sec("defensive anti-aim", "Right")
  local k = new_sec("kernel's resolver")
  for _, r in ipairs(head) do bind(r.k, add(a, r, cfg[r.k])) end
  for _, r in ipairs(rlist) do bind(r.k, add(k, r, cfg[r.k])) end
  for _, r in ipairs(alist) do
    local sec = (r.g == "d" or r.k == "def") and d or a
    ed[#ed + 1] = { k = r.k, g = r.g, el = add(sec, r, r.d) }
  end
end

pages.chat = function()
  local m = new_sec("send", "Right")
  for _, l in ipairs(wrap("if you want to get your own chat hosted on my server, dm kernel1ptrace(discord)", 32)) do m:new_label(l) end
  clive[#clive + 1] = { k = "on", el = m:new_toggle("enabled", cc.on) }
  local box = m:new_textbox("message", "type here", "")
  m:new_button("send", function()
    local ok, text = pcall(function() return box:get() end)
    if ok and type(text) == "string" and chat_send(text) then
      pcall(function() box:set("", true) end)
    end
  end)
  chat_build()
end

local function sync()
  for _, c in ipairs(live) do
    local ok, v = pcall(function() return c.el:get() end)
    if ok and v ~= nil then cfg[c.k] = v end
  end
  for _, c in ipairs(clive) do
    local ok, v = pcall(function() return c.el:get() end)
    if ok and type(v) == "boolean" then cc[c.k] = v end
  end
end

local shown
local function apply_page()
  local cur = page_dd:get()
  if cur == shown then return end
  sync()
  shown = cur
  for _, s in ipairs(secs) do pcall(function() s:destroy() end) end
  secs, live, ed, loaded = {}, {}, {}, nil
  clive = {}
  if pages[cur] then pages[cur]() end
end
apply_page()

local size = Vector3.new(2.5, 4.8, 2.5)
local ws = game:GetService("Workspace")
local plrs = game:GetService("Players")
local white = rgb(255, 255, 255)
local trk = {}

local rp_cache, rp_time = nil, -10

local function rays()
  local now = os.clock()
  if rp_cache and now - rp_time < 0.3 then return rp_cache end
  local ign = {}
  for _, p in ipairs(plrs:GetPlayers()) do
    if p.Character then ign[#ign + 1] = p.Character end
  end
  local rp = RaycastParams.new()
  rp.FilterType = Enum.RaycastFilterType.Exclude
  rp.FilterDescendantsInstances = ign
  pcall(function() rp.RespectCanCollide = true end)
  rp_cache, rp_time = rp, now
  return rp
end

local function floor_y(x, y, z, rp)
  local ok, hit = pcall(ws.Raycast, ws, Vector3.new(x, y + 3, z), Vector3.new(0, -500, 0), rp)
  return ok and hit and hit.Position.Y or nil
end

local function est_vel(s, pos, now)
  local h = s.hist
  h[#h + 1] = { pos = pos, t = now }
  while #h > 2 and now - h[2].t > 0.12 do table.remove(h, 1) end
  local dt = now - h[1].t
  if dt < 0.04 then return s.vel end
  s.vel = s.vel:Lerp((pos - h[1].pos) / dt, 0.35)
  return s.vel
end

local function get_vel(e, s, pos, now)
  local est = est_vel(s, pos, now)
  if cfg.apiv then
    local ok, v = pcall(entity.get_player_velocity, e)
    if ok and v and v.X == v.X and v.Y == v.Y and v.Z == v.Z and v.Magnitude < 300 then
      if v.Magnitude > 0.5 or est.Magnitude < 1 then return v end
    end
  end
  return est
end

local function trend(s, vel, ground, dt)
  local sp = math.sqrt(vel.X * vel.X + vel.Z * vel.Z)
  local a = 1 - math.exp(-12 * dt)
  if s.sp then s.dsp = s.dsp + ((sp - s.sp) / dt - s.dsp) * a end
  s.sp = sp
  s.stop = ground and s.dsp < -40 and sp <= 22.044
  if sp > 3 then
    local hd = math.atan2(vel.Z, vel.X)
    if s.hd then
      local diff = (hd - s.hd + math.pi) % (2 * math.pi) - math.pi
      s.om = s.om + (math.clamp(diff / dt, -12, 12) - s.om) * a
    end
    s.hd = hd
  else
    s.hd = nil
    s.om = s.om * 0.8
  end
end

local function sim(s, pos, vel, ground, total, rp)
  local steps = math.max(1, math.ceil(total * 64 - 1e-6))
  local x, y, z = pos.X, pos.Y, pos.Z
  local vy = vel.Y
  local sp = math.sqrt(vel.X * vel.X + vel.Z * vel.Z)
  local dx, dz = 0, 0
  if sp > 0.5 then dx, dz = vel.X / sp, vel.Z / sp else sp = 0 end
  local om = s.om * cfg.curve / 100
  local lx, ly, lz = x, y, z
  local left = total

  for i = 1, steps do
    local h = math.min(1 / 64, left)
    left = left - h

    if ground then
      vy = 0
      if sp > 0 then
        if s.stop then
          sp = math.max(0, sp - math.max(sp, 8) * 26 * h)
        else
          sp = math.max(0, sp - math.max(sp, 5.552) * 5.5 * h)
          if sp > 1 and sp < 20.044 then sp = sp + math.min(20.044 - sp, 5.72 * h * 20.044) end
        end
      end
    else
      if sp > 29 then sp = math.max(29, sp - (sp - 29) * 2 * h) end
      if om ~= 0 and sp > 0 then
        local c, sn = math.cos(om * h), math.sin(om * h)
        dx, dz = dx * c - dz * sn, dx * sn + dz * c
      end
      y = y + (vy - 29.35 * h) * h
      vy = vy - 58.7 * h
    end

    x = x + dx * sp * h
    z = z + dz * sp * h

    if i % 8 == 0 or i == steps then
      local wx, wz = x - lx, z - lz
      local wl = math.sqrt(wx * wx + wz * wz)
      if wl > 0.05 then
        local from = Vector3.new(lx, ly, lz)
        local ok, hit = pcall(ws.Raycast, ws, from, Vector3.new(wx, 0, wz), rp)
        if ok and hit then
          local back = math.max((hit.Position - from).Magnitude - 1.25, 0)
          x, z = lx + wx / wl * back, lz + wz / wl * back
          sp = 0
        end
      end

      local f = floor_y(x, math.max(y, ly), z, rp)
      if f then
        local gy = f + s.off
        if ground then
          if y - gy > 1.2 then
            ground = false
            vy = 0
          elseif math.abs(y - gy) <= 2 then
            y = gy
          end
        elseif y <= gy and vy <= 0 then
          y = gy
          ground = true
          vy = 0
        end
      end
      lx, ly, lz = x, y, z
    end
  end
  return Vector3.new(x, y, z)
end

local function predict(dt)
  if not cfg.pred then
    trk = {}
    return
  end
  dt = dt and dt > 0 and math.max(dt, 0.004) or 0.016
  local now, ms = os.clock(), cfg.ms
  if cfg.lat then pcall(function() ms = ms + client.latency() * 1000 end) end
  local alpha = cfg.smooth <= 0 and 1 or 1 - math.exp(-cfg.smooth * dt)
  local rp, seen = rays(), {}

  for _, e in ipairs(entity.get_enemies()) do
    local ok, pos = pcall(function()
      if entity.is_alive(e) then return entity.get_player_position(e) end
    end)
    if ok and pos then
      seen[e] = true
      local s = trk[e]
      if not s then
        s = { hist = {}, vel = Vector3.new(0, 0, 0), om = 0, dsp = 0, off = 2.4 }
        trk[e] = s
      end
      local okg, g = pcall(movement.is_grounded, e)
      local ground = okg and g and true or false

      local vel = get_vel(e, s, pos, now)
      trend(s, vel, ground, dt)

      if not s.tgt or now - s.tgt_t >= 0.033 then
        if ground then
          local f = floor_y(pos.X, pos.Y, pos.Z, rp)
          if f then
            local o = pos.Y - f
            if o > 0.5 and o < 6 then s.off = s.off + (o - s.off) * 0.2 end
          end
        end
        s.tgt = sim(s, pos, vel, ground, ms / 1000, rp)
        s.tgt_t = now
      end
      local target = s.tgt
      if not s.box or (s.box - target).Magnitude > 60 then
        s.box = target
      else
        s.box = s.box:Lerp(target, alpha)
      end
      render3d.wire_box(CFrame.new(s.box), size, white, 0, true)
      render3d.line(pos, s.box, white, 1, 0.6, true)
    end
  end
  for e in pairs(trk) do
    if not seen[e] then trk[e] = nil end
  end
end

local accent = rgb(232, 160, 168)
local bg, edge = rgb(46, 46, 46), rgb(70, 70, 70)
local pbg, pbd = rgb(30, 30, 30), rgb(90, 90, 90)

local function frame(parent, x, y, w, h, col, round)
  local f = Instance.new("Frame")
  f.BorderSizePixel = 0
  f.Position = UDim2.new(x, 0, y, 0)
  f.Size = UDim2.new(w, 0, h, 0)
  if col then f.BackgroundColor3 = col else f.BackgroundTransparency = 1 end
  if round then
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(round, 0)
    c.Parent = f
  end
  f.Parent = parent
  return f
end

local function stroke(f, th, col)
  local s = Instance.new("UIStroke")
  s.Thickness = th
  s.Color = col
  s.Parent = f
  return s
end

local gui = Instance.new("ScreenGui")
gui.Name = "supportive_rectangle"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.DisplayOrder = 1000
gui.ZIndexBehavior
